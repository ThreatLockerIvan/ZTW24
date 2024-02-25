![ZTW Logo](../../Assets/Hacking_Labs_graphics_ztw_logo_med_1.png)

- [Intro](#intro)
- [Overview of Nmap](#overview-of-nmap)
- [How to install Nmap](#how-to-install-nmap)
  - [Windows](#windows)
  - [Linux](#linux)
    - [APT, yum](#apt-yum)
    - [RPM](#rpm)
- [What Options should we use](#what-options-should-we-use)
- [When to use what](#when-to-use-what)
- [References](#references)

# Intro

Welcome to the nmap class, We will cover:

1. How Nmap works
2. How to Install it
3. How to Use it
4. What most options do

# Overview of Nmap

Nmap is one of the most used tool in any hacker tool kit. Ask any pentester and
they would say that use Nmap in the last week. But what is Nmap, Nmap is a
network scanner created by Gordon Lyon. Nmap is used to discover hosts and
services on a computer network by sending packets and analyzing the responses.

# How to install Nmap

Whats the point of talking about Nmap and everything about it, if you can't
even figure out how to install the thing.

Luckly Nmap make this part very easy, All we have to do go the the Nmap
download page and install the version you need for your OS.

Nmap Site https://nmap.org/download.html

## Windows

For the windows install, You hit the download link for `nmap-<VERSON.NUMBER>-setup.exe`
and it should install, Once you download the install just follow the install
guild.

> Note: You can get Zenmap, buts its no longer being updated.
> Zenmap is the GUI version of Nmap

## Linux

The linux install can be easy if you already have a package in your repository.

### APT, yum

You can install Nmap with one of these commands

```bash
sudo apt install nmap
```

OR

```bash
sudo yum install nmap
```

Once its done installing, you should have Nmap installed

### RPM

To install with `RPM` you need to get the `RPM` package
https://nmap.org/download.html#linux-rpm

Once you have the package install on your linux machine you can use the following:

```bash
sudo rpm -i /path/to/package_name.rpm
```

Example

```bash
 sudo rpm -i /home/bob/Downloads/nmap-7.94-1.x86_64.rpm
```

Once its done installing, you should have Nmap installed

# What Options should we use

Nmap is now installed now you can use the `man` command to read the manual.
Or you can got to https://nmap.org/book/man.html for a super in detail notes.

# When to use what

Nmap is a tool with many different use cases. Because of this, nmap can be used
in a large variety of different contexts.

For our first example, lets say you need to find all the existing machines on a
network. You would normally use a ping scan to find them, and nmap can fit this
usage.

- `-sn` - This option tells Nmap not to do a port scan after host discovery, and
  only print out the available hosts that responded to the host discovery probes.
  This is often known as a “ping scan”, but you can also request that traceroute
  and NSE host scripts be run. This is by default one step more intrusive than
  the list scan, and can often be used for the same purposes. It allows light
  reconnaissance of a target network without attracting much attention. Knowing
  how many hosts are up is more valuable to attackers than the list provided by
  list scan of every single IP and host name.

```bash
nmap -sn 192.168.1.0/24 192.168.1.* 192.168.2.1-254 8.8.8.8
```

If you dont need a ping scan, you can discover information on a target with nmap
too.

- `-p` - Selects the ports that nmap will scan for. `-p-` scans all ports from
  1-25565 for all TCP connections.
- `-sV` - Collects information on the service that is running on each port. This
  can be helpful When a common port like port 80 is running a different service
  like SSH. It can also help identify the version of the application running on
  that port.
- `-O` - Attempts to discover what operating system the machine on using. It
  will also attempt to discover the version of said operating system with varying
  success.

```bash
nmap -p- -sV -O <ip>
```

If you already have information on your target, you can gather more information using the `--script` argument. Nmap comes with several very useful scripts. You can get more information about each script using `--script-help`. You can also find all of the scripts that nmap provides in `/usr/share/nmap/scripts/`.

- `--script` - Runs a script on the target

```bash
nmap --script=<script_location> <ip>
```

# References

- **Nmap Download link:** https://nmap.org/download.html
- **Nmap man book:** https://nmap.org/book/man.html
