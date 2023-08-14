# Common TwinCAT BSD Commands

## Restart
```bash
shutdown -r now
```

## Restore point
### Create
```bash
doas restorepoint create your-name
```
### Show points
```bash
restorepoint status
```

### Restoring
```bash
doas restorepoint rollback
```

## System Update
Before updating or upgrading your system make sure to have a proper restore point created beforehand
### Update packages
```bash
doas pkg upgrade
```
### Update major version
```bash
doas pkg update -f && doas pkg upgrade
```
after upgrading, dont forget to restart system with
```bash
doas shutdown -r now
```

## Firewall
### Stop
```bash
doas service pf stop
```
### Start
```bash
doas service pf start
```
### Add Modbus-TCP to firewall
```bash
doas ee /etc/pf.conf
```
Add the following to the file,
```
pass in quick proto tcp to port 502 keep state
```
Restart the firewall with new rule
```bash
doas pfctl -f /etc/pf.conf
```

## Change user password
```bash
passwd
```

# File System

## Overview of important TwinCAT/BSD directories.

| Directory             | Description                                                                                   |
|-----------------------|-----------------------------------------------------------------------------------------------|
| /                     | Root directory and top-level directory hierarchy.                                              |
| /bin/                 | Basic user applications for single-user and multi-user environments.                           |
| /boot/                | Kernel, drivers, programs and configuration files for the boot process.                        |
| /dev/                 | Device nodes, which can be used to access hardware directly, for example.                      |
| /etc/                 | System-relevant scripts and configuration files.                                              |
| /home/                | Link to /usr/home, where the home directories of the users are located.                        |
| /mnt/                 | Empty directory; usually serves as a mount point for USB sticks, for example.                  |
| /root/                | Home directory of the superuser root.                                                         |
| /sbin/                | Basic system applications for single-user and multi-user environments.                         |
| /usr/                 | Unix system resources, contains most of the user applications.                                 |
| /usr/bin              | General applications.                                                                         |
| /usr/include/         | Contains header files for C compilers.                                                        |
| /usr/local/           | Local programs and libraries, i.e., software installed by a user, such as software unrelated to the basic FreeBSD system itself. |
| /usr/local/bin/       | Mainly Beckhoff applications                                                                  |
| /usr/local/etc/       | Configuration files, TwinCAT directory with TwinCAT Functions and PLC project.                 |
| /usr/local/include/   | Including ADS header files TcAdsDef.h and TcAdsAPI.h                                           |
| /usr/sbin/            | System applications that are executed by the user.                                            |
| /var/                 | Variable files, i.e. temporary files with changing content such as log files.                   |
| /var/log/             | Contains system log files.                                                                     |
