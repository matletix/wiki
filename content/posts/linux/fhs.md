+++
title = "The Filesystem Hierarchy Standard (FHS)"
date = "2023-02-28T20:30:30+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["linux", "fhs", "filesystem"]
keywords = ["", ""]
description = "Many UNIX systems use a set of standardized directories and files for organizing their file system. Maintained by the Linux Foundation, the **Filesystem Hierarchy Standard (FHS)** is the reference describing these conventions."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++


Many UNIX systems use a set of standardized directories and files for organizing
their file system. Maintained by the Linux Foundation, the **Filesystem Hierarchy
Standard (FHS)** is the reference describing these conventions.

## Conventions

- **/home** : user home directories
- **/root** : root user's home directory. Not in /home because /home is often on a 
separate partition or network share and it needs to be accessible in single user mode
- **/boot** : boot loader, kernel files
- **/etc** : text configuration files
- **/opt** : self-contained 3rd party packages
- **/media** : content of the removable media
- **/mnt** : temporary mounted filesystems
- **/tmp** : temporary files, usually deleted on reboot
- **/bin** : essential binaries needed before /usr is mounted (eg. during early boot
stage or in single user mode)
- **/sbin** : essential binaries for which superuser privileges (root) are required
- **/lib** : library files needed by essential binaries
- **/usr** : things not needed for single user mode
  - **/usr/bin** : general system-wide binaries
  - **/usr/sbin** : general binaries for system administration
  - **/usr/lib** : library files needed by general binaries
  - **/usr/local** : local to this computer. Not managed by the system packages
    - **/usr/local/bin** : locally installed programs
    - **/usr/local/sbin** : locally installed programs for system administration
- **/var** : files that change a lot. Often on its own partition to avoid crashing
the system if it fills up
  - **/var/log** : log files
  - **/var/tmp** : temporary files, do persist accross reboots, although they are
often deleted in a site-specific manner (but less frequently than /tmp).
  - **/var/spool** : store various type of spooled data. "spooling" refers to the
  process of temporarily holding data in a queue or buffer while it's being
  processed or transmitted.
    - **/var/spool/cron** : spool files for the cron job scheduler
    - **/var/spool/mail** : spool files for local email messages
    - **/var/spool/cups** : spool files for print jobs
- **/dev** : hardware represented as files
- **/dev/shm** : virtual filesystem for in-memory temporary files (shared memory)
- **/proc** : virtual filesystem used to expose kernel data structures to user-space
programs. Mostly read-only and process-centric. Contains one folder per process.
  - **/proc/cpuinfo** : info about CPU on the system
  - **/proc/meminfo** : info about memory usage on the system
  - **/proc/kcore** : displays the actual system memory. Only file in proc with a size
  - **/proc/modules** : list of kernel loaded modules
  - **/proc/cmdline** : passed boot parameters
  - **/proc/version** : kernel version and time of compilation
  - **/proc/swaps** : status of swap partitions
- **/sys**: virtual filesystem used to expose device information to user-space 
programs. Read/write filesystem, allows configuration of devices by writing to a file.
In general, the information provided in /proc is process-centric and relates to the
system as a whole, while the information provided in /sys is device-centric and 
relates to the hardware and drivers in the system.

## Additional Notes
- Nowadays, `/bin`, `/sbin` and `/lib` often are links to `/usr/bin`, `/usr/sbin` and
`/usr/lib`. This allows improved compatibility with other *NIXes. Historically the
/bin and /sbin directories were used to mount the usr partition. This job is nowadays
done by initramfs, and splitting the directories therefore no longer serves any purpose.

## Links

- https://refspecs.linuxfoundation.org/fhs.shtml
- https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard
- https://askubuntu.com/questions/308045/differences-between-bin-sbin-usr-bin-usr-sbin-usr-local-bin-usr-local
- https://unix.stackexchange.com/questions/266517/why-is-bin-a-symbolic-link-to-usr-bin
- https://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge/
