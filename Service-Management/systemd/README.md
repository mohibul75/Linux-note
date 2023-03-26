# Service Management in Linux

Systemd is a system and service manager for Linux operating systems. It is responsible for managing the startup and shutdown of the system, as well as managing the services that run on the system. 

Systemd plays a key role in the initialization process of Linux operating systems. It is responsible for starting the initial processes that are required to bring the system up and running, and for managing the dependencies between them. Additionally, systemd provides a range of advanced features, such as process management, resource tracking, and logging, that enable system administrators to optimize the performance and reliability of their systems. With systemd, you can easily manage the entire lifecycle of a service, from its startup to its shutdown, and ensure that it runs smoothly and reliably over time. Some initialization process of Linux operating systems are:

- SysV init
- BSD init
- LSB init
- systemd

First three process are outdated and are replaced by systemd. One of the main advantages of Systemd is that it can parallelize the startup of services, which speeds up the boot process.The main reason why systemd has replaced the previous three initialization systems (SysV init, BSD init, and LSB init) is because systemd is written in C, while the earlier systems were written in shell scripts. This difference in programming language makes systemd significantly faster and more efficient than its predecessors, as it can parallelize the startup of services and handle dependencies between them in a more optimized way. Moreover, systemd provides a more centralized and streamlined approach to managing system services, which makes it easier to troubleshoot issues and improve system performance.

To check the initialization process on your Linux system, you can use a variety of commands. Some of them are:
```sh
stat /sbin/init
stat /proc/1/exe
cat /proc/1/comm
 ```

 If the ***/run/systemd*** directory exists on your system, you can be sure that your initialization process is being managed by systemd

During the boot process, systemd is the first process that is started by the Linux operating system, and its process ID is always 1. To verify that systemd is the initialization process on your system, you can check its process ID using the following command:
```sh
ps -p 1
```
If the process name ends with 'D', it means that the process is a daemon, which runs in the background. Knowing the process ID of systemd can be useful for troubleshooting purposes or for monitoring system performance.

## Commands to inspect systemd
```sh
ps -ef   # to see all the processes
pstree ps <PID>  # to see the process tree with child process
ps -e0 pid.comm # to see the  #to see the process id and associated commands
man ps  # ps command documentation
```

***systemctl*** is a command-line utility for controlling the systemd system and service manager. It allows you to manage system services, units, targets, and sockets, as well as control the system state, including starting and stopping services, enabling or disabling them at boot time, and viewing their status.

```sh
systemctl
systemctl -all
systemctl list-unit-files
systemctl list-unit-files --type=service
systemctl list-unit-files --type=service --state=enabled
```
