# Configuring rsyslog to listen on localhost for UDP/TCP events

**Target OS:** CentOS 7

By default, rsyslog is configured to not listen on any UDP or TCP ports. If UDP/TCP is required it should be limited to who can communicate with it for security reasons.

For this example, we will make the assumption that we need to accept UDP messages on the local machine from a service called haproxy. The service will be configured to send syslog events to 127.0.0.1 using 514/UDP. We want to restrict access to rsyslog to only bind on the localhost adapter. Below are the instructions to configure this setup.

**Step 1:**

Using an editor open `/etc/rsyslog.d/haproxy.conf`. This file does not exist and is named after the service that is using it. By placing our configurations in this file we can be assured that updates for `/etc/rsyslog.conf` can be applied without having to compare the old and new files. 

**Step 2:**

Add the following to the configuration file:

```
$ModLoad imudp
$UDPServerAddress 127.0.0.1
$UDPServerRun 514
```

*NOTE: The order of `$UDPServerAddress` and `$UDPServerRun` is required. If these two are swapped the Server Address will not be applied.*

**Step 3:**

Restart the rsyslog service using `systemctl restart rsyslog`.

**Step 4:**

Confirm that rsyslog is only listening on the port and server address specified by running `sudo ss -tlnup | grep rsyslog`. Sudo access is required to confirm the process name. 

Example Response:

```
[root@host /etc/rsyslog.d]# ss -tlnup | grep rsyslogd
udp    UNCONN     0      0      127.0.0.1:514                   *:*                   users:(("rsyslogd",pid=10360,fd=3))
```
