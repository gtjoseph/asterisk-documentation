---
search:
  boost: 0.5
title: MeetmeJoin
---

# MeetmeJoin

### Synopsis

Raised when a user joins a MeetMe conference.

### Syntax


```


    Event: MeetmeJoin
    Meetme: <value>
    User: <value>
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


* `Meetme` - The identifier for the MeetMe conference.<br>

* `User` - The identifier of the MeetMe user who joined.<br>

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

* [AMI Events MeetmeLeave](/Asterisk_16_Documentation/API_Documentation/AMI_Events/MeetmeLeave)
* [Dialplan Applications MeetMe](/Asterisk_16_Documentation/API_Documentation/Dialplan_Applications/MeetMe)


### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 