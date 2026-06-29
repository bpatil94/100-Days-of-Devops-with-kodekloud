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


Step 1 — Identify the server in which the apache service is failing.( check for all 3 app servers

```
ssh <app-server-user>@<app-server-name>
```
or go with telnet command 

```
telnet [hostname_or_IP] [port]

```
