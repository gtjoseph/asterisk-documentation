---
search:
  boost: 0.5
title: MonitorStop
---

# MonitorStop

### Synopsis

Raised when monitoring has stopped on a channel.

### Syntax


```


    Event: MonitorStop
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

* [AMI Events MonitorStart](/Asterisk_16_Documentation/API_Documentation/AMI_Events/MonitorStart)
* [Dialplan Applications StopMonitor](/Asterisk_16_Documentation/API_Documentation/Dialplan_Applications/StopMonitor)
* [AMI Actions StopMonitor](/Asterisk_16_Documentation/API_Documentation/AMI_Actions/StopMonitor)


### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 