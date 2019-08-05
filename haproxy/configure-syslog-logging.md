# Configure syslog logging

**Target OS:** CentOS 7<br>
**HAProxy Version:** 1.5.18

haproxy by default is configured to send syslog events to localhost:514 using UDP. All events are sent with the *local2* facility. 

This documentation assumes that the local rsyslog daemon will be configured to accept these events locally and store them at `/var/log/haproxy.log`. Additionally, automatic log rotation will be configured to occur daily and to keep 15 copies (10 is the default). 

**Step 1:**

Confirm that haproxy is configured to send syslog events to **127.0.0.1:514** as **local2** facilities. 

```
[root@lb haproxy]# head /etc/haproxy/haproxy.cfg -n 13
...
global
    log         127.0.0.1 local2
```

**Step 2:**

Create a new rsyslog configuration file called **/etc/rsyslog.d/haproxy.conf** and apply the following settings:

```
# Enable UDP on localhost only
$ModLoad imudp
$UDPServerAddress 127.0.0.1
$UDPServerRun 514

# Route all local2 messages to haproxy.log
local2.*        /var/log/haproxy.log
```

Optionally, update `/etc/rsyslog.conf` and disable local2 events from being logged to /var/log/messages as shown below:

```
*.info;mail.none;authpriv.none;cron.none;local2.none                /var/log/messages
```

**Step 3:**

Restart the rsyslog service to apply settings using `systemctl restart rsyslog`.

**Step 4:**

Edit the following file `/etc/logrotate.d/haproxy` and apply the changes below. 

```
/var/log/haproxy.log {
    daily
    rotate 15
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
        /bin/kill -HUP `cat /var/run/rsyslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

**Step 5:**

Assuming the haproxy service is running a new **haproxy.log** file should be present in **/var/log**. Additionally, **/var/log/messages** should no longer receive any events from the haproxy service. 

