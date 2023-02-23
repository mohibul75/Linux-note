# lsof
---------------------
The lsof command stands for "list of open files", and on Linux/Unix, everything is considered a file, including directories and network sockets. The lsof command can be used to display a list of all open files and the processes that have them open, including regular files, directories, network sockets, and more.

The output format of the lsof command includes information such as the process ID (PID), user ID (UID), file descriptor (FD), type of file, and file name/path. Here's an example of what the output might look like:

COMMAND   PID   USER  FD   TYPE DEVICE SIZE/OFF   NODE NAME
bash     1234   user cwd    DIR   8,3     4096      2 /root
bash     1234   user rtd    DIR   8,3     4096      2 /root
bash     1234   user txt    REG   8,3  1036616 968988 /bin/bash
bash     1234   user mem    REG   8,3    61912  24856 /lib/x86_64-linux-gnu/libnss_files-2.31.so


To see a complete list of available options, you can use the lsof -h command. This will display the help menu, which provides a detailed description of each option, along with any required or optional arguments.

***An interesting use of lsof command:*** This lsof command will display a list of all processes that are listening for incoming network connections on any port. By default, the output will include information such as the process ID (PID), user ID (UID), protocol, and local and remote addresses. 
    
* -i: Shows information only about network connections.
* -P: Prevents the command from translating port numbers to port names.
* -n: Disables the conversion of network numbers to hostnames or usernames, which makes the output faster.

```sh
lsof -i -P -n
```
This command will show a list of open network connections and the processes which are opened.

```sh
lsof -i:<port_number>
```
This command is used to display information about the process that is using the specific port on a Linux system.

```sh
lsof -i -P -n | grep LISTEN
```
We can be used this command to list all the listening port .

