---
search:
  boost: 0.5
title: MixMonitorStart
---

# MixMonitorStart

### Synopsis

Raised when monitoring has started on a channel.

### Syntax


```


    Event: MixMonitorStart
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

### Class

CALL
### See Also

* [AMI Events MixMonitorStop](/Asterisk_16_Documentation/API_Documentation/AMI_Events/MixMonitorStop)
* [Dialplan Applications MixMonitor](/Asterisk_16_Documentation/API_Documentation/Dialplan_Applications/MixMonitor)
* [AMI Actions MixMonitor](/Asterisk_16_Documentation/API_Documentation/AMI_Actions/MixMonitor)


### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 