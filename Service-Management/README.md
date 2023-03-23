# Service Management
## systemd 
- systemctl
- journalctl
- Init
- process management
- Network management(networkd)
- Login management(logind)
- Logs(Journald)

Any entity managed by systemd is called a unit. A systemd unit can manage a:
- service
- Socket
- Device
- Mountpoint or Automount point
- Swap file
- Partition
- Startup target
- watch file system path
- Group of externally created processes

systemd unit file locations
- /lib/systemd/system  - standard systemd unit files(your distro maintainers)
- /usr/lib/systemd/system -from locally installed package(e.g via apt-get)
- /run/systemd/system -transient unit files
- /etc/systemd/system - this is where you're putting your custom unit files

A sample unit file
```sh
[Unit]
Description= A very simple service created by Dave
After= network-up.target

[Service]
ExecStart= /usr/local/bin/daveprogram

[Install]
WantedBy= multi-user.target
```

## init.d
The init daemon is the primary process in a Linux system, responsible for launching other daemons, threads, services, and processes. These additional components are started via scripts located in the /etc/init.d directory. As such, /etc/init.d serves as the configuration database for the init process. By adding a new script to this directory, it will be automatically started by init. This makes /etc/init.d a crucial location for managing the startup and configuration of services in a Linux system.


