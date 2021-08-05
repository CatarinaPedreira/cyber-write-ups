# TryHackMe Vulnversity 

## Description:
As part of the "Getting started" rooms, this room is focused on teaching about active recon, web app attacks and privilege escalation.

## Level:
Easy

04 August 2021, Catarina Pedreira
-------------------------------

## Known information:
- Vulnerable machine title: VulnUniversity
- Vulnerable machine IP: 10.10.162.194 (for my machine, substitute with the correct IP for yours)


## Steps:
- Start your AttackBox (or connect to the target machine through OpenVPN)

Reconnaissance:

- We will be using nmap. Nmap is a very powerful tool used by hackers to discover hosts or services on a computer network. The task describes nmap, and has a table explaining some of the flags commonly used.

- In my case, I will run the command "nmap -sV 10.10.162.194", meaning that nmap will discover hosts or services on this IP address (the address of the vulnerable machine), and try to determine the version of the services running.

- After running the previous command, we get a list of open ports, services associated with those ports, and their versions. 

- The list returns 6 open ports (the answer to the second question).

- We can see that squid http proxy is running on port 3128, and it's version is 3.5.12 (the answer to the third question)

- Then, we execute the command "nmap -p-400 10.10.162.194". From the table, we can see that the flag -p- scans all open ports. 
But there is a 400 right in front of it. It can't be scanning just port 400 since a modified version of the flag does that (-p 400). Let's see what happens :)

The output shows 3 open ports, and 397 closed ports. Makes sense! The command is probably scanning for all ports, up to 400. Quick google search and it checks out. The answer to this question is... you guessed it, 400.

- Onto the next question. From the table, we see that the flag -Pn disables host discovery and just scans for open ports. When we execute the command "nmap -n 10.10.162.194", we get a list of ports, their states services, but we don't get any info about the host (as we did in prior commands). So, in isolation, the flag -n disables host discovery.

Looking at the question, by disabling host discovery (and with the help of the hint since not all network concepts are entirely fresh in my mind), the -n flag is disabling DNS - Domain Name System - a hierarquical system that translates IP addresses to domain names. When we type a web address URL in a browser, DNS servers resolve it and return a computer-friendly translation - the IP address.

- For the next question, we already know (from execution of past commands), through Service Info, that the host runs a unix/linux operating system. We just don't know which one exactly. 
To do this, we can execute nmap with the flag -A provided in the table. The output of the command hints that the operating system is Ubuntu in the version information of the ssh service in port 22 (which, indeed, is the correct answer). 

- Finally, by looking at the output of the previous command "nmap -sV 10.10.162.194", we can see that there is an Apache webserver running on port 3333, which is the answer to the last question.

