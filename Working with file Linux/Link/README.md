# Link in Linux
In Linux, creating links is a way to reference or access a file or directory from another location. There are two types of links commonly used: symbolic links and hard links.


## Symbolic Links (Soft Links):

- Symbolic links, often referred to as soft links, are references to other files or directories by name. They act as pointers to the original file or directory.
- Symbolic links can span across different file systems or even point to non-existing locations.
- To create a symbolic link, you can use the ln -s command. Here's the basic syntax:

```sh
ln -s <target_file> <link_name>
```

Replace <target_file> with the path to the original file or directory and <link_name> with the desired name/location of the symbolic link.

## Hard Links:

- Hard links are additional names (links) associated with the same underlying file or directory. They point directly to the file's inode, rather than its name.
- Hard links can only be created within the same file system, and they cannot reference directories.
- To create a hard link, you can use the ln command without any additional options. Here's the basic syntax:

```sh
ln <target_file> <link_name>
```

Replace <target_file> with the path to the original file and <link_name> with the desired name/location of the hard link.

[Hard link vs soft link](./link.png)

However, it's important to note that deleting the original file does not affect hard links, as they are separate names pointing to the same file. Deleting a symbolic link does not delete the target file either; it simply removes the link itself. But when you delete the orginal file or directory , symbolic link will not work.