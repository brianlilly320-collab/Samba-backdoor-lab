UnrealIRCd Backdoor Exploitation – Metasploitable 2

Project Overview

A practical penetration testing lab demonstrating the identification and assessment of an UnrealIRCd backdoor vulnerability using Metasploit.

Lab Environment

- Attacker OS: Kali Linux
- Target OS: Metasploitable 2
- Target IP: 192.168.56.103
- Attacker IP: 192.168.56.102
- Service: UnrealIRCd (Port 6667)
- Tool: Metasploit Framework

Objective

To understand the security impact of the UnrealIRCd backdoor vulnerability in an isolated virtual lab.

Results

- Vulnerable service identified.
- Vulnerability check successful.
- Command shell session established.
- "whoami" returned root.
- System information verified using "uname -a".

Security Impact

The vulnerability demonstrated the possibility of unauthorized remote command execution with root privileges.

Conclusion

This project provided practical experience in vulnerability assessment, exploitation verification, and understanding the importance of securing vulnerable network services.

Environment: Authorized educational lab using Metasploitable 2.