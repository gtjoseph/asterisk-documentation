---
search:
  boost: 0.5
title: AgentRingNoAnswer
---

# AgentRingNoAnswer

### Synopsis

Raised when a queue member is notified of a caller in the queue and fails to answer.

### Syntax


```


    Event: AgentRingNoAnswer
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
    DestChannel: <value>
    DestChannelState: <value>
    DestChannelStateDesc: <value>
    DestCallerIDNum: <value>
    DestCallerIDName: <value>
    DestConnectedLineNum: <value>
    DestConnectedLineName: <value>
    DestLanguage: <value>
    DestAccountCode: <value>
    DestContext: <value>
    DestExten: <value>
    DestPriority: <value>
    DestUniqueid: <value>
    DestLinkedid: <value>
    Queue: <value>
    MemberName: <value>
    Interface: <value>
    RingTime: <value>

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

* `DestChannel`

* `DestChannelState` - A numeric code for the channel's current state, related to DestChannelStateDesc<br>

* `DestChannelStateDesc`

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

* `DestCallerIDNum`

* `DestCallerIDName`

* `DestConnectedLineNum`

* `DestConnectedLineName`

* `DestLanguage`

* `DestAccountCode`

* `DestContext`

* `DestExten`

* `DestPriority`

* `DestUniqueid`

* `DestLinkedid` - Uniqueid of the oldest channel associated with this channel.<br>

* `Queue` - The name of the queue.<br>

* `MemberName` - The name of the queue member.<br>

* `Interface` - The queue member's channel technology or location.<br>

* `RingTime` - The time the queue member was rung, expressed in seconds since 00:00, Jan 1, 1970 UTC.<br>

### Class

AGENT
### See Also

* [AMI Events AgentCalled](/Asterisk_16_Documentation/API_Documentation/AMI_Events/AgentCalled)


### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 