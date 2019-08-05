# WORK IN PROGRESS
# Working with iptables for docker

**Target OS:** CentOS 7<br>
**Docker Version:** 1.13.1 (default provided on CentOS repositories)

**Required Packages:**
*  iptables
*  iptables-services

This article discusses how to secure containers that have ports exposed by limiting which IP addresses can communicate with the container ports. By default, Docker configures IP filter rules in the DOCKER chain that does not limit who can access the exposed ports. To implement these limits a few extra steps are required to insert new iptable rules that restrict access further. This is especially important when you are in a shared environment where the IP space may not be as restricted. 

Before applying any rules, CentOS 7 will need require a few additional configurations to enable iptables 

