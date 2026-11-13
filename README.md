Penetration Test Report: 

Kali Linux vs. Metasploitable2

. Summary

For this project, I tested the security of one computer in a safe practice lab. My attacking machine ran Kali Linux, and my target was Metasploitable2, a Linux system that is built to be vulnerable so people can learn hacking safely.

My goals were to:

Find which computers were active on the network
See which services the target had open
Find out whether I could break in and take control

Result: I got full control of the target. I broke in through a weakness in Samba, a file-sharing service, and ended up with root (administrator) access. This was all done in an isolated network, so no real systems were ever at risk.

. Test Details
Item	Details
Type of test	Internal network test in an isolated lab
My machine (attacker)	Kali Linux, IP 192.168.56.101
Target machine	Metasploitable2, IP 192.168.56.102
Network	192.168.56.0/24 (VirtualBox host-only network)
Tools	ip, nmap 7.95, Metasploit Framework 6.4.84-dev

. How I Did It

I followed four simple steps:

Find hosts: see which devices are online
Scan services: see what is running on the target
Exploit: use a weakness to get in
Verify: confirm what level of access I got

. What I Found

 step 1: Checking my own machine. Before scanning, I checked my network setup. Kali had two connections: one for the internet (10.0.2.15) and one for the lab network (192.168.56.101).

Step 2: Finding live hosts. I scanned the lab network with nmap and found four active devices: the gateway (.1), two VirtualBox devices (.100 and .102), and my own machine (.101). I picked .102 as my target because it looked like the lab VM.

Step 3: Scanning the target. A service scan showed 23 open services, which is a lot for one computer. Some of the important ones were:

Port	Service	Version
21	FTP	vsftpd 2.3.4
22	SSH	OpenSSH 4.7p1
23	Telnet	Linux telnetd
139, 445	Samba (file sharing)	Samba 3.x–4.x
1524	Bindshell	Already-open root backdoor
3306	MySQL	5.0.51a
5432	PostgreSQL	8.3.0–8.3.7
6667	IRC	UnrealIRCd

Old software versions, an open backdoor, and outdated remote-access tools all showed that this machine was built to be weak. I chose Samba as my way in.

Step 4: Breaking in. I used Metasploit to find a module for a Samba flaw called “username map script” (CVE-2007-2447). This flaw is from 2007 and has long been fixed. It lets an attacker run commands on the server by sending special characters in the username. I set the target and my own address, ran the module, and it worked. The target opened a connection back to my machine, giving me a command shell.

Step 5: Checking my access. I ran a few basic commands to see who I was on the target:

id showed uid=0(root)
whoami showed root
uname -a confirmed it was the Metasploitable Linux system

I had root access straight away, without needing a password or any extra steps. That means a full takeover of the target from the network.

5. Risk Levels 

The system has two critical risks: a Samba command execution flaw that lets anyone take full control remotely, and an open root backdoor on port 1524 that gives direct root access with no login. It also has two high risks, since the old vsftpd 2.3.4 may contain a known backdoor and Telnet, rsh, and rlogin send passwords as plain text. Finally, many other outdated services pose a medium to high risk by giving attackers more ways in.


6. Recommendations

To fix these problems, I would:

1. Update or remove old Samba versions, and keep installing security updates regularly
2. Turn off old services like telnet, rsh, and rlogin, and use SSH with key-based login instead
3. Remove the backdoor on port 1524 and find out how it got there
Separate old or unsupported systems from important networks
4.Scan regularly and keep a patching schedule so outdated software gets caught early

7. Conclusion

This test showed how easily a system can be taken over when it runs old software. With one well-known, long-fixed flaw, I went from having no access to full root control. On a real network, this would need to be fixed immediately, and the team would also need to look at why such outdated software was installed in the first place.
