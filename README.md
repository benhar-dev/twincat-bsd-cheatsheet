# Common TwinCAT BSD Commands
Further commands can be found [here](https://infosys.beckhoff.com/english.php?content=../content/1033/twincat_bsd/6884717195.html&id=3494672470280562782)

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

## Package Management

### See what packages are installed
```bash
pkg info
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
doas pkg install tcbsd-upgrade
doas tcbsd-upgrade major
```
after upgrading, dont forget to restart system with
```bash
doas shutdown -r now
```

## Write Filter
The dataset zroot/ROOT/default, which contains most of the system and TwinCAT, is protected against write accesses when the write filter is active. No other datasets are covered by the write filter. For example, user files can still be persistently stored at /usr/home or log files at /var/log, even if the rest of the system is reset after a restart.

### Enable
```bash
doas service bwf enable
```

### Disable
```bash
doas service bwf disable
```

### Example, Exclude TwinCAT Boot Folder
Exceptions for the write filter can be defined by creating new zfs datasets, since only the dataset zroot/ROOT/default is protected from write accesses; all other system datasets, including newly created datasets, are excluded from the protection.

1. Delete the current boot directory and all it's contents
   ```bash
   doas rm -rf /usr/local/etc/TwinCAT/3.1/Boot/*.
   ```
2. Create a new ZFS dataset named zroot/usr/TwinCAT-Boot and mount it at /usr/local/etc/TwinCAT/3.1/Boot.
   ```bash
   doas zfs create -o mountpoint=/usr/local/etc/TwinCAT/3.1/Boot zroot/usr/TwinCAT-Boot
   ```
Any data stored in this dataset will be accessible at this location and is excluded from the write filter

### Reversal of the above example, 
In case you need to reverse the changes made in the above example and revert the directory to being managed by `zroot/ROOT/default`, you can follow these steps:

1. **Unmount the new dataset**:
   ```bash
   doas zfs unmount zroot/usr/TwinCAT-Boot
   ```
2. Destroy the new dataset
   ```bash
   doas zfs destroy zroot/usr/TwinCAT-Boot
   ```
3. Recreate the directory within the original dataset
   ```bash
   doas mkdir -p /usr/local/etc/TwinCAT/3.1/Boot
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

## Setting Isolated cores
The following command will set one core as being isolated.  You can see core informtion using `TcCoreConf`.
```bash
TcCoreConf –s 1
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
