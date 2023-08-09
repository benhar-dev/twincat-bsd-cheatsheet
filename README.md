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
