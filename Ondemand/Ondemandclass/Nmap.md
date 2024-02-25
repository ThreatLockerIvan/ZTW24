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

For the windows install, You hit the download link for 
`nmap-<VERSON.NUMBER>-setup.exe` and it should install, Once you download the
install just follow the install guild.

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

Once you have the package install on your linux machine you can use the 
following:

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
s
# References
  - **Nmap Download link:** https://nmap.org/download.html
  - **Nmap man book:** https://nmap.org/book/man.html