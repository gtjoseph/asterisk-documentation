---
search:
  boost: 0.5
title: SoftHangupRequest
---

# SoftHangupRequest

### Synopsis

Raised when a soft hangup is requested with a specific cause code.

### Syntax


```


    Event: SoftHangupRequest
    Channel: <value>
    ChannelState: <value>
    ChannelStateDesc: <value>
    CallerIDNum: <value>
    CallerIDName: <value>
    ConnectedLineNum: <value>
    ConnectedLineName: <value>
    Language: <value>
    AccountCode: <value>
    Context: <value>
    Exten: <value>
    Priority: <value>
    Uniqueid: <value>
    Linkedid: <value>
    Cause: <value>

```
##### Arguments


* `Channel`

* `ChannelState` - A numeric code for the channel's current state, related to ChannelStateDesc<br>

* `ChannelStateDesc`

    * `Down`

    * `Rsrvd`

    * `OffHook`

    * `Dialing`

    * `Ring`

    * `Ringing`

    * `Up`

    * `Busy`

    * `Dialing Offhook`

    * `Pre-ring`

    * `Unknown`

* `CallerIDNum`

* `CallerIDName`

* `ConnectedLineNum`

* `ConnectedLineName`

* `Language`

* `AccountCode`

* `Context`

* `Exten`

* `Priority`

* `Uniqueid`

* `Linkedid` - Uniqueid of the oldest channel associated with this channel.<br>

* `Cause` - A numeric cause code for why the channel was hung up.<br>

### Class

CALL
### See Also

* [AMI Events HangupRequest](/Asterisk_16_Documentation/API_Documentation/AMI_Events/HangupRequest)
* [AMI Events Hangup](/Asterisk_16_Documentation/API_Documentation/AMI_Events/Hangup)


### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 