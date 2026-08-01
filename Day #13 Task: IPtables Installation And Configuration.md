# Day 13: IPtables Installation And Configuration

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8086 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

1. Install iptables and all its dependencies on each app host.

2. Block incoming port 8086 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

**What is iptables?**
This is a Linux tool, that controls network traffic coming in, going out or even passing through the server. Using this tool, we define the rules that implements which input/output requests should be accepted/denied [dropped] through which port, through which ip.
- How does a basic rule look like?
  ```
  iptables -A <CHAIN> <conditions> -j <TARGET>
  ```
  1. -A <CHAIN> → Append the rule to a chain (INPUT, OUTPUT, or FORWARD)
  2. <conditions> → Optional filters like protocol, source IP, destination port, interface
  3. -j <TARGET> → What to do with matching packets (ACCEPT, DROP, REJECT)

- A typical example would be something similar to this:
  ```
  sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
  ```
  1. -A INPUT → add rule to the incoming traffic chain
  2. -p tcp → only TCP traffic
  3. --dport 22 → only traffic destined for port 22 (SSH)
  4. -j ACCEPT → allow the packet

This means that it would allow incoming SSH connection from everywhere. Now, do note that whatever rules you add, the priority would be a top-down approach. What that means is, that it starts with the first rule and if the first rule is fulfilled/satisfied, it discards the remaining rules.


## Step-1: SSH into the app-server and install the required packages.
```
ssh <app-server-user>@<app-server-name>
```
- Install the iptables package and the required dependencies (iptables-services).
```
sudo dnf install -y iptables iptables-services
```
- After successfully installing the above packages, start the iptables services.
```
sudo systemctl enable iptables
```
Created symlink /etc/systemd/system/multi-user.target.wants/iptables.service → /usr/lib/systemd/system/iptables.service.
```
sudo systemctl start iptables
```

## Step-2: View the existing rules and analyze what’s allowed and denied.
- To view the existing rules.
```
sudo iptables -L -n --line-numbers
```
As per the existing rules, this is what it roughly translates to [A brief summary]:
- In the INPUT chain, it essentially allows all the incoming traffic as per RULE #3.
- In the FORWARD chain, all the traffic is rejected.
- In the OUTPUT chain, everything is allowed.
- Specific mentions like SSH (port 22) or ICMP are present but redundant due to the blanket ACCEPT.

## Step-3: Add the new rules as defined in the task.
We need rules for 5 things:
- First, we need to ensure that we won’t be kicked out of our current/active SSH session.
- Ensure that the server is allowed to talk to itself through localhost (127.0.0.1). This is called the Loopback rule.
- We need to allow the requests to go through the Load Balancer server through the defined port in question.
- Whatever is already enabled, we need to preserve all such communications.
- And lastly, we need to enable DROP by default, so that no other incoming requests other than the above mentioned are accepted.

- !/bin/bash
- Safe iptables setup for CentOS Stream (remote-friendly)
- OS: CentOS Stream
- App server IP: <app-server-01>,<app-server-02>,<app-server-03>
- LBR IP: <LBR-ip>
- Apache listening on port <port-defined in question>
- Default INPUT policy: DROP

## --- Step 1: Determine your current SSH client IP ---
MY_IP=$(echo $SSH_CLIENT | awk '{print $1}')

## --- Step 2: Insert safe allow rules at the top ---
echo "Adding rules safely without flushing..."

## Allow SSH from current session first (top priority)
sudo iptables -C INPUT -p tcp -s $MY_IP --dport 22 -j ACCEPT 2>/dev/null || \
sudo iptables -I INPUT 1 -p tcp -s $MY_IP --dport 22 -j ACCEPT

## Allow LBR access to port 5000
sudo iptables -C INPUT -p tcp -s <LBR-ip> --dport <apache-port> -j ACCEPT 2>/dev/null || \
sudo iptables -I INPUT 2 -p tcp -s <LBR-ip> --dport <apache-port> -j ACCEPT

## Allow loopback traffic
sudo iptables -C INPUT -i lo -j ACCEPT 2>/dev/null || \
sudo iptables -I INPUT -j ACCEPT -i lo

## Allow established and related connections
sudo iptables -C INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT 2>/dev/null || \
sudo iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

## --- Step 3: Ensure default DROP policy for INPUT ---
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

## --- Step 4: Add a final catch-all drop if not already present ---
sudo iptables -C INPUT -j DROP 2>/dev/null || \
sudo iptables -A INPUT -j DROP


Validate whether all the rules are successfully applied.

 <img width="838" height="456" alt="image" src="https://github.com/user-attachments/assets/ebe899ff-b18b-4cbb-9179-581fea03ff9f" />


## Step-4: Save/Persist the rules to be applied even after system reboot.
You need to save the rules so that, even when the app-server is rebooted, the rules are still applied and followed.
## --- Step 5: Save rules persistently ---
```
sudo service iptables save
```
iptables: Saving firewall rules to /etc/sysconfig/iptables: [  OK  ]

## You can view the file in the /etc/sysconfig/iptables to actually verify if the rules are saved.
```
 cat /etc/sysconfig/iptables
```
<img width="697" height="613" alt="image" src="https://github.com/user-attachments/assets/11247e3b-87ad-4310-a18b-e933e0f51226" />

## Step-5: Verify if the rules are successfully applied.

To check if the rules are implemented and applied successfully, we need to try the curl command from the load balancer server and from any other server.

## SSH into the load balancer server.
thor@jumphost ~$ ssh loki@stlb01
[loki@stlb01 ~]$ curl http://<app-server-ip/name>:<port>
## You should be able to see the content.

## SSH into any other server [You can try from the jumphost server too]. Your request will be timed out.
thor@jumphost ~$ curl http://<app-server-ip/name>:<port>
curl: (28) Failed to connect to stapp01 port <port>: Connection timed out


Repeat the same step for all the three app servers.


