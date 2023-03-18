+++
title = "File Permissions in Linux"
date = "2023-03-18T14:29:15+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["linux", "permission", "access", "control"]
description = "File systems often implement a way to manage permissions on their files. In Linux, most of the supported filesystems like ext2/3/4 or Btrfs implement both the *POSIX file system permissions* and the more granular *Access Control Lists* (ACLs). Additional *File attribute flags* also are available, although their support varies. And to complicate matters further, *Linux Security Modules* (LSM) like SELinux, AppArmor, Smack or TOMOYO, add another layer of access control."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
toc = true
+++

File systems often implement a way to manage permissions on their files. In Linux, most of
the supported filesystems like ext2/3/4 or Btrfs implement both the *POSIX file system permissions*
and the more granular *Access Control Lists* (ACLs). Additional *File attribute flags*
also are available, although their support varies. And to complicate matters further, *Linux 
Security Modules* (LSM) like SELinux, AppArmor, Smack or TOMOYO, add another layer of
access control.

## The POSIX file system permissions
Files in this system possess 3 types of permissions :
 - *read*
 - *write*
 - *execute*

These 3 permissions are applied for each class of users :
 - the *owner* of the file
 - users that are part of the *group* of the file
 - other users

Additionally, three modes can be applied globally (not for each class of users) on each file :
 - *setuid*
 - *setgid*
 - *sticky*

### Effects of permissions and modes
In Unix, everything is a file, but the permissions do not mean the same thing if the file
is a regular file or if it's a directory.

For a *regular file* :
 - *read* : access the file's content
 - *write* : truncate or modify the file's content
 - *execute* : execute the file by the system
 - *setuid* : only has an effect if the *execute* permission is also set, and, on most systems,
   if the executable is a binary file (ignored if the executable is a *shell script*). Then, the
   executable is run with the privileges of the file's owner.
 - *setgid* : same as *setuid*, but the executable is run with the privileges of the
   file's *group*.
 - *sticky* : ignored on most system

For a *directory* :
 - *read* : list the *names* (only) of the files in the directory
 - *write* : modify entries in the directory (create/delete/rename its files). To be effectively
   realized, these operations also need the *execute* bit to be set. The *write* permission
   bit alone is meaningless otherwise.
 - *execute* : access the content and metadata of a given file it the directory, enter the directory
   (with `cd`) or pass through it to access subfolders.
 - *setuid* : ignored on most systems
 - *setgid* : files created inside it inherit its group ownership, rather than the primary
   group of the processes creating the files.
 - *sticky* : restrict deleting or renaming a file inside the directory to its owner, to the 
   owner of the directory and to the root user. *write* permission for other users is not
   sufficient.

### Representation of permissions
Permissions on a given file can be viewed with the `ls` command :

```bash
$ ls -l /path/to/a/file
-rw-r--r-- 1 mrollet mrollet 31154 14 oct.  15:09 file
```

### Umask

## File attribute flags
TODO: representation with `ls`

## Access Control Lists (ACLs)
TODO: representation with `ls` (`+`)

## Linux Security Modules (LSM)
TODO: representation with `ls` (`.`)

## Links
 - [File-system permissions on Wikipedia](https://en.wikipedia.org/wiki/File-system_permissions)
 - [Setuid/setgid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)
 - [Sticky bit on Wikipedia](https://en.wikipedia.org/wiki/Sticky_bit)