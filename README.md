# nagios-plugins-asterisk
simple nagios plugins for monitoring asterisk peers, queues and other stuff

## INSTALLATION:
Clone the repo into /somewhere/path
```
git clone https://github.com/nixhr/nagios-plugins-asterisk
```
Create links in `/usr/lib/nagios/plugins` or wherever your plugins reside, or
copy the files directly into plugins directory.

### If using it locally 
Add to nagios `commands.cfg`:
```
define command{
        command_name    asterisk_queue_members
        command_line    sudo /usr/lib/nagios/plugins/asterisk_queue_members $ARG1$
}
define command{
        command_name    asterisk_peer
        command_line    sudo /usr/lib/nagios/plugins/asterisk_peer $ARG1$
}
```
### If using nrpe 
Add to `nrpe_local.cfg`:
```
command[asterisk_queue_members]=sudo /usr/lib/nagios/plugins/asterisk_queue_members $ARG1$
command[asterisk_peer]=sudo /usr/lib/nagios/plugins/asterisk_peer $ARG1$
```

Add to `/etc/sudoers`:
```
nagios ALL=(ALL) NOPASSWD: /usr/lib/nagios/plugins/asterisk*
```

### Configure service on remote nagios server like:
```
define service {
        host_name                       hostname
        service_description             description
        check_command                   check_nrpe!asterisk_peer!peername
        use                             generic-service
}
```
