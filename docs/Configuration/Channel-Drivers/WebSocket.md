# WebSocket **DRAFT**

## Background

The WebSocket Channel Driver (chan_websocket) is designed to ease the burden on ARI application developers with getting media in and out of Asterisk.  The ARI /channels/externalMedia REST endpoint already has two other channel drivers  available (AudioSocket and RTP) but they require binary packet manipulation (RTP especially) and both require that the app developer handle the timing of sending packets to asterisk.  chan_websocket requires neither.

## Features

* Send and receive media using most Asterisk codecs.
* TLS is supported.
* Send arbitrary packet lengths to Asterisk.  The channel driver will break them up into appropriately sized frames (see notes below though).
* No need to time your own packet transmits.
* Silence is automatically generated when no packets have been received from the app.
* The channel driver can accept incoming websocket connections _from_ your app as well as make outgoing connections _to_ your app.
* Although the driver is targetted at ARI ExternalMedia users, it's not tied to ARI and can be used directly from the Dial dialplan app.

## Details

### Outgoing Connections

Outgoing connections require you to pre-configure a websocket client in the `websocket_client.conf` config file (see details below).  Once done, you can reference the connection in a dial string.

```ini
[default]
exten = _x.,1,Dial(WebSocket/connection1/c(ulaw))
```

This would connect to your application's websocket server using the client named `connection1` and using the `ulaw` codec.  When your server accepts the connection, the websocket channel will be answered and media will begin to flow.

### Incoming Connections

Incoming connections must be made to the global Asterisk HTTP server using the `media` URI but you must still 'Dial" the channel using the special `INCOMING` connection name.

```ini
[default]
exten = _x.,1,Dial(WebSocket/INCOMING/c(ulaw))
```

The websocket channel will be created immediately and the `MEDIA_WEBSOCKET_CONNECTION_ID` channel variable will be set to an ephemeral connection id which must be used in the URI your application will connect to Asterisk with.  For example `/media/32966726-4388-456b-a333-fdf5dbecc60d`.  When Asterisk accepts the connection, the websocket channel will be answered and media will flow.

Attempting to connect a second time with the same connection id will result in a `409 Conflict` response.

### Media Path

#### "Chunking" The Data

Media sent from Asterisk to your application is simply streamed in BINARY websocket messages.  The message size will be whatever the internal Asterisk frame size is.  For ulaw/alaw for instance, Asterisk will send a 160 byte packet every 20ms.  This is the same as RTP except the messages will contain raw media with no RTP or other headers.  You could stream this directly to a file or other service.

Media sent _to_ Asterisk _from_ your app is a bit trickier because chances are that the media you send Asterisk will need to go out to a caller in a format that is both framed and properly timed.  For example, what should happen if you're using the ulaw codec but you send only 100 bytes? That's 60 bytes short of the data needed to create 20ms worth of audio so what is Asterisk supposed to do with it?  If Asterisk sends a short frame to the core it will surely result in the connected user hearing pops or clicks.  If you send Asterisk 200 bytes, Asterisk can make a full 20ms packet with it but what should it do with the leftover 40 bytes?  The answer is pretty simple but it does require a bit of extra work on your part to ensure the caller hears a good audio stream.

When the websocket channel is created, a `MEDIA_WEBSOCKET_OPTIMAL_PACKET_SIZE` channel variable will be set that tells you the amount of data Asterisk needs to create a good 20ms frame using the codec you specified in the dialstring.  If you send a websocket message with a length that's exactly that size or some even multiple of that size, the channel driver will happily break that message up into the correctly sized frames and send one frame to the core every 20ms with no leftover data.  If you send an oddly sized message though, the extra data that won't fill a frame will be dropped.  However...

If you need to send a file or a buffer received from an external source like an AI agent, it's quite possible that the buffer size won't be an even multiple of the optimal size.  In this case, the app can send Asterisk a `START_BULK_XFER` TEXT websocket message. This tells the channel driver to make as many full frames as it can from the next BINARY message it receives _and save the leftover data_.  When the following BINARY message comes in, the channel driver will grab the previous leftover data then take as much data from the new message as it needs to make a full frame, send that to the core, then continue pulling data from the new message making as many full frames as it can then save the leftover data.  That process will continue until the app sends Asterisk a `END_BULK_XFER` TEXT message. When the channel driver receives that, it'll take whatever data is leftover, append silence to it to make up a full frame and send that to the core.

So why can't Asterisk just do that process all the time and dispense with the TEXT messages?  Well, let's say the app sends a message with an odd amount of data and the channel driver saves off the odd bit.  What happens if you don't send any data for a while?  If 20ms goes by and the channel driver doesn't get any more data what is it supposed to do with the lefover?  If it appends silence to make a full frame and sends it to the core, then the app sends more data after 30ms, the caller will hear a gap in the audio.  If the app does that a lot, it'll be a bad experience for ther caller.  It also takes work on the channel_driver's part to keep saving off the leftover data and prepending it to the next buffer.

#### Max Message Size and Flow Control

Chances are that your app will be sending data faster to Asterisk than Asterisk will be sending out to a caller so there are some rules you need to follow to prevent the channel driver from consuming excessive memory...

/// warning
The maximum websocket message size the underlying websocket code can handle is 65500 bytes.  Attempting to send a message greater than that length will result in the websocket being closed and the call hungup!
///

* The maximum number of frames the channel driver will keep in its queue waiting to be sent to the core is about 1000.  That's about 20 seconds of audio with a 20ms packetization rate.  When the queue gets to about 900 frames, the channel driver will send a `MEDIA_XOFF` TEXT message to the app.  The message the app sent just prior to receiving `MEDIA_XOFF` will be processed in its entirety even if the resulting frames cause the queue to reach 1000 but any data the app sends after that will probably be dropped.  When the queue backlog drops down below about 800 frames, the channel driver will send a `MEDIA_XON` TEXT message at which time it's safe to start sending data again.

* The app can send a `GET_QUEUE_LENGTH` TEXT message to Asterisk which will cause the channel driver to reply with a `QUEUE_LENGTH <frames in queue>` TEXT message.

* The app can send a `MEDIA_FLUSH` TEXT message to Asterisk which will cause the channel driver to immediately discard any frames in the queue and send silence to the core until more data comes in from the app.  This could be useful if an automated agent detects the caller is speaking and wants to interrupt a prompt it already replied with.

/// warning
You must ensure that the control messages are sent as TEXT messages.  Sending them as BINARY messages will cause them to be treated as audio.
///

## Configuration

All configuration is done in the common [websocket_client.conf](/Latest_API/API_Documentation/Module_Configuration/res_websocket_client) file shared with ARI Outbound WebSockets.  That file has detailed information for configuring websocket client connections.  There are a few additional things to know though...

* You only need to configure a connection for outgoing websocket connections. Incoming connections (those with the special `INCOMING` connection id in the dial string) are handled by the internal http/websocket servers.

* chan_websocket can only use `per_call_config` connection types.  `persistent` websocket connections aren't supported for media.

* Never try to use the same websocket connection for both ARI and Media. "Bad things will happen"®

## Dial Strings

The full dial string is as follows:

```
WebSocket/<connection_id>/<options>
```

* **WebSocket**: The channel technology.
* **&lt;connection_id&gt;**: For outgoing connections, this is the name of the pre-defined client connection from websocket_client.conf.  For incoming connections, this must be the special `INCOMING` id.
* **&lt;options&gt;**: The only option currently supported is `c(<codec>)`. If not specified, the first codec from the caller's channel will be used.  Having said that, if your app is expecting a specific codec, you should specify it here or you may be getting audio in a format you don't expect.

Examples:

```
Dial(WebSocket/connection1/c(alaw)
Dial(WebSocket/INCOMING/c(slin16)
```

