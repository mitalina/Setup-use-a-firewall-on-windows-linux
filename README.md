# Setup-&-use-a-firewall-on-windows-linux

## Introduction of Firewall in Computer Network:

A firewall is a network security device either hardware or software-based which monitors all incoming and outgoing traffic and based on a defined set of security rules it accepts, rejects, or drops that specific traffic. It acts like a security guard that helps keep your digital world safe from unwanted visitors and potential threats.

o Accept: allow the traffic

o Reject: block the traffic but reply with an “unreachable error”

o Drop: block the traffic with no reply

A firewall is a type of network security device that filters incoming and outgoing network traffic with security policies that have previously been set up inside an organization. A firewall is essentially the wall that separates a private internal network from the open Internet at its very basic level.


## 1.Open firewall configuration tool:

o Open Windows Defender Firewall: Go to Control Panel > System and Security > Windows Defender Firewall.

o Turn on Windows Defender Firewall: Ensure the firewall is enabled for both private and public networks.

## Linux Firewall (UFW/iptables):

### UFW:- (Uncomplicated Firewall):

### Install UFW:
        sudo apt-get install ufw 
                    
### Enable UFW: 
        sudo ufw enable.


## 2.List current firewall rules. 

 To list current firewall rules:

### UFW:

1.     sudo ufw status
     
2.     sudo ufw status <numbered>
     

### iptables:

1.     sudo iptables -n -L (list all rules)

2.     sudo iptables -n -L INPUT (list INPUT chain rules)

3.     sudo iptables -n -L OUTPUT (list OUTPUT chain rules)

![firewall_list](https://github.com/user-attachments/assets/4662829b-bd1a-4699-a8e0-b9007f0cac0b)

## 3.Add a rule to block inbound traffic on a specific port (e.g., 23 for Telnet).

To add a rule to block inbound traffic on a specific port:

## UFW:

      sudo ufw deny 23 

## iptables:

1.     sudo iptables -A INPUT -p tcp --dport 23 -j DROP


2. Save the rule:
 
       sudo iptables-save 

## Reload/Restart:

### 1. UFW:
      sudo ufw reload

### 2. iptables:
      sudo service iptables restart (or sudo systemctl restart iptables)

![ufw deny](https://github.com/user-attachments/assets/12b676ca-18fd-46ab-930e-16b5040b0845)


## 4.Test the rule by attempting to connect to that port locally or remotely.

To test the rule:

### Local Testing:

1. Should timeout or connection refused:
 
         telnet localhost 23
   
3. Should show connection failed:
 
         nc -zv localhost 23 

### Remote Testing:

1. Should timeout or connection refused:

         telnet <IP> 23
   
3. Should show connection failed:

         nc -zv <IP> 23

![ufw test](https://github.com/user-attachments/assets/7f22a216-99d8-45ab-a50f-2108677a3d8a)

## 5.Add rule to allow SSH (port 22) if on Linux.

To add a rule to allow SSH (port 22) on Linux:

### UFW:

1.      sudo ufw allow ssh

2.      sudo ufw allow 22/tcp

### iptables:

1.      sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

2.      sudo iptables-save

### Reload/Restart:

### UFW:

        sudo ufw reload

### iptables:

        sudo service iptables restart (or sudo systemctl restart iptables)

This allows incoming SSH connections on port 22.

![ufw ssh](https://github.com/user-attachments/assets/8759a128-f265-45ea-9daf-f0fab266f499)

## 6.Remove the test block rule:

### UFW:
(get rule number from sudo ufw status numbered)

1.      sudo ufw delete <rule number> 

2.      sudo ufw delete deny 23

### iptables:
(get rule number from sudo iptables -n --list INPUT --line-numbers)
             
 1.       sudo iptables -D INPUT <rule number> 

             
 2.       sudo iptables-save

![ufw remove](https://github.com/user-attachments/assets/6add5a43-d689-4cda-9638-f8ec395d46df)


## 7.To verify the rule allow/removal:

### UFW:

        sudo ufw status

### iptables:

        sudo iptables -n -L INPUT

Check if the block rule for port 23 is removed.

![firewall verify](https://github.com/user-attachments/assets/0856adfc-7010-468d-a1c8-de7374a4ca1a)

## 8.Summarize how firewall filters traffic.

A firewall filters traffic by:

o Inspecting packets: Analyzing incoming and outgoing network traffic.

0 Applying rules: Matching traffic against predefined rules (allow, block, or redirect).

o Controlling access: Allowing or blocking traffic based on:
    
- Source/destination IP addresses
    
- Ports (e.g., HTTP, SSH)
    
- Protocols (e.g., TCP, UDP)
    
- Packet contents

### Firewalls can be configured to:

o Block unwanted traffic: Prevent unauthorized access.

o Allow necessary traffic: Enable legitimate communication.

By controlling traffic flow, firewalls enhance network security and protect against threats.

