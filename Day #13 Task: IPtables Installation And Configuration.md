# Day 13: IPtables Installation And Configuration

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8086 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

1. Install iptables and all its dependencies on each app host.

2. Block incoming port 8086 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.


- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

" What is iptables? "
This is a Linux tool, that controls network traffic coming in, going out or even passing through the server. Using this tool, we define the rules that implements which input/output requests should be accepted/denied [dropped] through which port, through which ip.
