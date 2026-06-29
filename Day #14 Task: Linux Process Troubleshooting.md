# Day #14 Task: Linux Process Troubleshooting.

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 5004 on all app servers.



Let’s clearly define what’s expected of us to do in the task given:
- One of the app servers has Apache down:
A monitoring system flagged that Apache is not available on one of the app hosts. There are usually multiple app servers (e.g., stapp01, stapp02, stapp03). We need to identify, on which app server is the apache service not running or is in the failed state.
- Fix Apache on the faulty host:
Make sure the service is running.It doesn’t matter if it serves content — only that the service is up.
- Apache must use port 8085 on ALL app servers:
Even if the service is running, it may be running on the wrong port. So you must ensure the configuration (Listen directive) is correct on all hosts and is using the port 8085 only.


## Step 1 — Identify the server in which the apache service is failing.( check for all 3 app servers)

```
ssh <app-server-user>@<app-server-name>
```
or go with telnet command ( The telnet command in Linux is a networking utility used to establish unencrypted TCP connections to a remote host.)

```
telnet [hostname_or_IP] [port]

```
## Step 2 - Verify why the service is failing and apply the fix.

On inspecting the logs, we can see that the address is already in use. Which means that some other process is using this port 5004. We need to identify which process is using the port and decide how we want the process to stop using the port. If you’ve read my article: KodeKloud Engineer Day 12: Linux Network Services., you’ll notice that this is quite similar.

note: try to look over [systemd] 
(98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:<port>
no listening sockets available, shutting down
AH00015: Unable to open logs

Check which process is already using the port.

```
sudo netstat -tulnp | grep <port>
```

or use network statistics ( is  a command line utilities used to monitor network connections)

```
sudo netstat -tulnp
```
or socket statistics  ( is also a command line utilities used to monitor network connections)
```
sudo ss -tulnp
```

You can see that the sendmail process is using the <port> defined in the problem statement. Since we’ve already covered moving the process to another port, in this article, we shall stop the service so that the port is free for another process to use.

```
sudo systemctl stop sendmail
sudo systemctl status sendmail
```


