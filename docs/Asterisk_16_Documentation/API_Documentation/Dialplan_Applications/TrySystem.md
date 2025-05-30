---
search:
  boost: 0.5
title: TrySystem
---

# TrySystem()

### Synopsis

Try executing a system command.

### Description

Executes a command by using system().<br>

Result of execution is returned in the **SYSTEMSTATUS** channel variable:<br>


* `SYSTEMSTATUS`

    * `FAILURE` - Could not execute the specified command.

    * `SUCCESS` - Specified command successfully executed.

    * `APPERROR` - Specified command successfully executed, but returned error code.

### Syntax


```

TrySystem(command)
```
##### Arguments


* `command` - Command to execute<br>

    /// warning
Do not use untrusted strings such as **CALLERID(num)** or **CALLERID(name)** as part of the command parameters. You risk a command injection attack executing arbitrary commands if the untrusted strings aren't filtered to remove dangerous characters. See function **FILTER()**.
///



### Generated Version

This documentation was generated from Asterisk branch 16 using version GIT 