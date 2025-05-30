---
title: SMS
pageid: 4260046
---

The SMS application
===================

SMS() is an application to handles calls to/from text message capable phones and message centres using ETSI ES 201 912 protocol 1 FSK messaging over analog calls.

Basically it allows sending and receiving of text messages over the PSTN. It is compatible with BT Text service in the UK and works on ISDN and PSTN lines. It is designed to connect to an ISDN or DAHDI interface directly and uses FSK so would   
 probably not work over any sort of compressed link (like a VoIP call using GSM codec).

Typical applications include:-

1. Connection to a message centre to send text messages - probably initiated via the manager interface or "outgoing" directory
2. Connection to an POTS line with an SMS capable phone to send messages - probably initiated via the manager interface or "outgoing" directory
3. Acceptance of calls from the message centre (based on CLI) and storage of received messages
4. Acceptance of calls from a POTS line with an SMS capable phone and storage of received messages

##### Arguments to sms():

* First argument is queue name
* Second is options:
	+ a: SMS() is to act as the answering side, and so send the initial FSK frame
	+ s: SMS() is to act as a service centre side rather than as terminal equipment
* If a third argument is specified, then SMS does not handle the call at all, but takes the third argument as a destination number to send an SMS to
* The forth argument onward is a message to be queued to the number in the third argument. All this does is create the file in the me-sc directory.
* If 's' is set then the number is the source address and the message placed in the sc-me directory.

All text messages are stored in /var/spool/asterisk/sms

A log is recorded in /var/log/asterisk/sms

There are two subdirectories called sc-me.<queuename> holding all messages from service centre to phone, and me-sc.<queuename> holding all messages from phone to service centre.

In each directory are messages in files, one per file, using any filename not starting with a dot.

When connected as a service centre, SMS(s) will send all messages waiting in the sc-me-<queuename> directory, deleting the files as it goes. Any received in this mode are placed in the me-sc-<queuename> directory.

When connected as a client, SMS() will send all messages waiting in the me-sc-<queuename> directory, deleting the files as it goes. Any received in this mode are placed in the sc-me-<queuename> directory.

Message files created by SMS() use a time stamp/reference based filename.

The format of the sms file is lines that have the form of key=value

Keys are :

* oa - Originating Address. Telephone number, national number if just digits. Telephone number starting with + then digits for international. Ignored on sending messages to service centre (CLI used)
* da - Destination Address. Telephone number, national number if just digits. Telephone number starting with + then digits for international.
* scts - Service Centre Time Stamp in the format YYYY-MM-DD HH:MM:SS
* pid - Protocol Identifier (decimal octet value)
* dcs - Data coding scheme (decimal octet value)
* mr - Message reference (decimal octet value)
* ud - The message (see escaping below)
* srr - 0/1 Status Report Request
* rp - 0/1 Return Path
* vp - mins validity period

Omitted fields have default values.

Note that there is special format for ud, ud# instead of ud= which is followed by raw hex (2 characters per octet). This is used in output where characters other than 10,13,32-126,128-255 are included in the data. In this case a comment (line starting ;) is added showing the printable characters

When generating files to send to a service centre, only da and ud need be specified. oa is ignored.

When generating files to send to a phone, only oa and ud need be specified. da is ignored.

When receiving a message as a service centre, only the destination address is sent, so the originating address is set to the callerid.

##### EXAMPLES

The following are examples of use within the UK using BT Text SMS/landline service.

This is a context to use with a manager script.

```
[smsdial]
; create and send a text message, expects number+message and
; connect to 17094009
exten => _X.,1,SMS(${CALLERIDNUM},,${EXTEN},${CALLERIDNAME})
exten => _X.,n,SMS(${CALLERIDNUM})
exten => _X.,n,Hangup

```

The script sends

```
 action: originate
 callerid: message <from>
 exten: to
 channel: Local/17094009
 context: smsdial
 priority: 1

```

You put the message as the name of the caller ID (messy, I know), the originating number and hence queue name as the number of the caller ID and the exten as the number to which the sms is to be sent. The context uses SMS to create the message in the queue and then SMS to communicate with 17094009 to actually send the message.

Note that the 9 on the end of 17094009 is the sub address 9 meaning no sub address (BT specific). If a different digit is used then that is the sub address for the sending message source address (appended to the outgoing CLI by BT).

For incoming calls you can use a context like this :-

```
[incoming]
exten => _XXXXXX_8005875290,1,SMS(${EXTEN:3},a)
exten => _XXXXXX_8005875290,n,System(/usr/lib/asterisk/smsin ${EXTEN:3})
exten => _XXXXXX_80058752[0-8]0,1,SMS(${EXTEN:3}${CALLERIDNUM:8:1},a)
exten => _XXXXXX_80058752[0-8]0,n,System(/usr/lib/asterisk/smsin ${EXTEN>:3}${CALLERIDNUM:8:1})
exten => _XXXXXX_80058752[0-8]0,n,Hangup

```

In this case the called number we get from BT is 6 digits (XXXXXX) and we are using the last 3 digits as the queue name.

Priority 1 causes the SMS to be received and processed for the incoming call. It is from 080058752X0. The two versions handle the queue name as 3 digits (no sub address) or 4 digits (with sub address). In both cases, after the call a script (smsin) is run - this is optional, but is useful to actually processed the received queued SMS. In our case we email them based on the target number. Priority 3 hangs up.

If using the CAPI drivers they send the right CLI and so the _800... would be _0800...
