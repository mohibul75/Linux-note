# lsof

lsof stands for list of open life. Linux/Unix consider everything as file and maintains folder. This command returns the list of open file in linux. The output format for lsof look like below:

COMMAND| PID |USER |FD |TYPE |DEVICE |SIZE/OFF |NODE |NAME

we can use multiple options with lsof. "lsof -h" command shows all the options.

***An interesting use of lsof command:*** lsof comand can be used to list all the listening port. To list all the listening port, we need to use the below options:
    
* -i : to select IPv4 files
* -P : no port names
* -n : no host name
