# Linux User
----------------------------------------
## some basic command about linux user information
------------------------------------------
|command|Description|
|---|---|
|logname| prints the current user login name|
|whoami|prints the current effective user|
|id|prints user ID and default group id and their associated name|
|who|Lists all log-in user|
|users|lists all user who have login session|
|finger|prints logged in user information in short way|
|last|Displays history of login|
|printenv|prints all environment variables known to user shell|

-------------------
* To see difference between logname and whoami command, use them with sudo.
-----------------
## User Management
--------------------------
|command|Description|
|----|---|
|useradd|to add a new user|
|usermod|to modify an user|
|userdel|to delete an user|
|passwd|to modify user's password|
|chfn|to change user's info|
|chsh|to change user's shell|

### Creating New User
```sh
sudo useradd userName
sudo useradd userName -d /home/username -s /bin/bash -g defaultGroup groupName
```
*****Usefuls options****
* -d (dir)      To set user's home directory
* -s (shell)    To set user's login shell
* -u (uid)      To set user's uid.
* -g (group)    To set user's default group to group
* -G            To set additional group

### Deleting a User

```sh
sudo userdel userName
sudo userdel -r userName 
```
***-r*** option for deleting user's all files.

### Modifying a User

```sh
sudo usermod -d /home/userName userName
```
***Useful Options***
* -d(dis)           TO change user's home directory
* -l                TO change user's login name
* -s                To change user's login shell
* -g                To change user's group
* -G                To make the user as aditional group member
* -L                To Disable(lock) the user account
* -U                To unlock the user account

### Changing Password of a User

```sh
sudo passwd userName
```

## Group Management
--------------------------
|command|Description|
|----|-----|
|group|Print all the groups of current user|
|groupadd|Create a group|
|groupmod|Modify a group|
|groupdel| Delete a group|
