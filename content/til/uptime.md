---
title: Linux uptime command
date: 2024-08-04T16:45:26+05:30
---

There are a few commands that I run whenever I login to an unknown server including `history`, `w`, `uptime`, `lscpu`, `htop` etc
One of the interesting commands of this bunch is `uptime`, which I only used to check how long has system been running. As the name suggests.
But it also prints a one line summary of system information, which initially used only to check how long the system had been running,
as the name suggests. However, it also prints a one-line summary of system information.

This is an example of uptime command

```
❯ uptime
 11:17:40 up 168 days, 21:46,  1 user,  load average: 0.02, 0.02, 0.00
```


This one line information has details about system up duration, number of users logged in and average system load for past 1,5 and 15 minutes.

Linux man pages describe load average as 
```
System load averages is the average number of processes that are either in a 
runnable or uninterruptable state.
A process in a runnable state is either using the CPU or waiting to use the 
CPU.
A process in uninterruptable state is waiting for some I/O access, eg waiting 
for disk.  The averages are taken over the three time intervals.  
Load averages are not normalized for the number of CPUs in a system, so a 
load average of 1 means a single CPU system  is loaded all the time while on 
a 4 CPU system it means it was idle 75% of the time.
```
What this means is that the `uptime` command gives us a quick and high-level idea of all processes in the system which are running or in a waiting state due to I/O operations or CPU time. This represents a combined utilization of all cores

For example, if I have a 2-core machine and the load average is 2.0, that means the CPU is completely utilized. If it's 1.0, that means 50% is utilized.
I have started relying on this command to get a cursory glance of system before drilling down on a machine for troubleshooting.

