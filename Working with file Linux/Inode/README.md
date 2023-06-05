# Inodes
In Linux, an inode (index node) is a data structure that stores metadata about a file or directory. It is a fundamental concept in the Linux file system.

Each file or directory in a Linux file system has a unique inode associated with it. The inode contains important information about the file, including its permissions, ownership, size, timestamps (such as creation, modification, and access), and pointers to the data blocks where the file's content is stored on the disk.

Some key characteristics of inodes in Linux are:

- Unique identification: Each inode has a unique identifier within the file system, allowing the system to distinguish between different files.

- Fixed size: Inodes have a fixed size, which varies depending on the file system. This size determines the maximum number of files and directories that can be stored on a given file system.

- Metadata storage: Inodes store metadata about files and directories but do not contain the actual file data itself. Instead, they store pointers to the data blocks where the file's content resides.

- Efficiency and performance: The use of inodes enables efficient file system operations. For example, accessing a file by its inode number is faster than searching for it by name.

- Limited storage: The number of inodes available on a file system is determined during its creation and is typically fixed. Running out of inodes can limit the number of files or directories that can be created, even if there is available disk space.

## Indoes working procedure

When a new file is created, it is assigned an inode number and a file name. An inode number is a unique number within that file system. Both name and inode number are stored as entries in a directory. Interally file name is first mapped with respective Inode number stored in a table.

## Necessary Command for Inode

```sh
ls -i # to see all inodes. 'i' for inodes
ls -li # to see in long listing format
```

```sh
stat <filename> # to show inode info for a specific file
```

```sh
df -i # to see all inodes information 
df -hi # to see in human readable format
```

