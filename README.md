# Common TwinCAT BSD Commands

## Restart
```bash
shutdown -r now
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
