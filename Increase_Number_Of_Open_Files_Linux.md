---
title: Increase Number of Open Files in Linux
layout: page
permalink: /increase-num-open-files-linux/
filename: Increase_Number_Of_Open_Files_Linux.md
--- 

# Introduction

The default configuration of open files in the system is usually 1024, which can be checked as below - 

```bash
# ulimit -n
1024
```
In order to temporarily increase it for the current session, we can use the ulimit command as below - 

```bash
# ulimit -n 65535
```

However, when you logout this setting will be back to 1024. To increase it permanently we need to perform the following - 

* Edit the `/etc/sysctl.conf file` 

```bash
# vi /etc/sysctl.conf
```
* Add the following line to it

```bash
fs.file-max = 65535
```
* Run the following to refresh with new config

```bash
# sysctl -p
```

Sometime this does not help, in that case we need to to increase the limits for the root user in the `limits.conf` file ny adding the following lines to it.

```bash
# vi /etc/security/limits.conf
....
....
root soft     nproc          65535    
root hard     nproc          65535   
root soft     nofile         65535   
root hard     nofile         65535
```

Next edit the pam common-session file and add the pam_limits module

```bash
# vi /etc/pam.d/common-session
....
....
session required pam_limits.so
```

Logout and login and then check the ulimit - 

```bash
# ulimit -n
65535
```
