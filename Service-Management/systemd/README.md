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
system-analyze # to see the total starting time
system-analyze # to see the starting time with breakdown
```

```sh
ps -ef   # to see all the processes
pstree ps <PID>  # to see the process tree with child process
ps -e0 pid.comm # to see the  #to see the process id and associated commands
man ps  # ps command documentation
```

***systemctl*** is a command-line utility for controlling the systemd system and service manager. It allows you to manage system services, units, targets, and sockets, as well as control the system state, including starting and stopping services, enabling or disabling them at boot time, and viewing their status.

```sh
systemctl # to see all the processes
systemctl -all # to see all the processes included unloaded, inactive, death processes
systemctl list-unit-files # to see the unit files status
systemctl list-unit-files --type=service # to see the unit files status which are services
systemctl list-unit-files --type=service --state=enabled # to see the unit files status which are enabled services
```

Systemd service action types include:

- start - to start a service
- stop - to stop a service
- restart - to restart a service
- reload - to reload the configuration of a service without stopping it
- enable - to enable a service to start automatically at boot time
- disable - to disable a service from starting automatically at boot time
- status - to check the status of a service
- try-restart - to restart a service only if it is already running
- reload-or-restart - to reload the configuration of a service and restart it if necessary
- reload-or-try-restart - to reload the configuration of a service and restart it only if it is already running

```sh
systemctl <service-action-type>  <service-name>
systemctl start nginx # example
```
The Systemd unit files that define how services are started, stopped, and managed are located in the directory /usr/lib/systemd. When a service is enabled, a symbolic link is created from /etc/systemd/system/<service-name> to the corresponding unit file in the usr/lib/systemd directory. This enables administrators to modify the configuration of a service without altering the original unit file. The systemctl command can be used to enable, disable, start, stop, or restart a service. Additionally, the mask and static commands can be used to prevent a service from starting or to declare a service as non-startable, respectively.

A masked service unit is a service that has been intentionally disabled and cannot be started. When a service is masked, systemd will prevent it from being started, either manually or automatically at boot time. This can be useful when you want to prevent a certain service from running on your system.

A static service unit, on the other hand, is a service that is not managed by systemd. Instead, it is included in the initramfs image and started by the init process before systemd takes control. This type of service is typically used for low-level system services that are required for booting the system, such as mounting file systems and setting up hardware.

### status of a service
There are several types of status that a systemd service can have:

- active (running): The service is currently running.
- inactive: The service is not currently running.
- failed: The service tried to start but encountered an error and stopped.
- activating: The service is in the process of starting up.
- deactivating: The service is in the process of shutting down.
- reloading: The service is reloading its configuration.
- maintenance: The service is running in a degraded state.

You can use the systemctl status command to check the status of a systemd service and get more detailed information about what's going on with the service.


#### Kill a Service
```sh
systemctl kill <service-name> # to stop a service with its all child process
systemctl kill -9 <service-name> # to kill a service with its all child process(-9 means nuclear kill)
```
