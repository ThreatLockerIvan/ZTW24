![](../Assets/ZTW_Logos_Text_only_light_wo_TL_500x185.png)

# Flipper Zero Unleashed

Welcome to the Flipper Zero Unleashed course. This class holds everything from an intro to intermediate level content.

# Guide

The following will be covered in this course:
 * Introduction to Flipper Zero
* Jail Breaking a Flipper Zero
* RFID Hacking
* Bad Bluetooth (BadKB)
* Bluetooth Impersonation Attacks

## Introduction to Flipper Zero

A Flipper Zero is a versatile and programmable multi tool that is commonly used for hacking. It's a handheld device equipped with various features that allow users to explore and understand different aspects of cybersecurity. Here's a breakdown of its key features:

1. **RFID/NFC Emulation:** The Flipper Zero can emulate RFID and NFC cards, allowing users to interact with contactless systems and understand their vulnerabilities.

2. **Bluetooth Hacking:** It has capabilities to analyze and manipulate Bluetooth signals, making it a useful tool for learning about wireless security protocols.

3. **Infrared Control:** With infrared capabilities, the Flipper Zero can be used for tasks like controlling TVs and other devices that use infrared communication.

4. **Tamagotchi-Like Interface:** The device has a playful Tamagotchi-like interface, making it more engaging for users while they learn about hacking and security.

6. **Open-Source and Customizable:** One of the notable aspects is that it's open-source, meaning users can modify and customize the software to suit their needs. This makes it a great tool for learning programming, cybersecurity, and ethical hacking.

## Jail Breaking a Flipper Zero

Jail Breaking a Flipper otherwise known as Flashing a Flipper typically refers to the process of loading or updating the firmware on the device. Firmware is the software that runs on embedded systems. Flashing is necessary when you want to install new features to extend your hacking abilities.

You may be asking how do I flash firmware onto my Flipper, is it easy? 
My answer to this yes it easy, if you use the Web Updater. Many GitHub repositories provide instant links to the web updater to install their firmware. Once you click these links all you have to do is plug in your flipper to your machine and click "Update".

**GitHub Repositories:**
> Below are the GitHub Repositories for the two most common Flipper Firmware's

Unleashed: [DarkFlippers/unleashed-firmware (github.com)](https://github.com/DarkFlippers/unleashed-firmware)

Extreme: [Flipper-XFW/Xtreme-Firmware (github.com)](https://github.com/Flipper-XFW/Xtreme-Firmware)

**Web Updater:**
> Below are Direct Links to the Flipper Labs Web Updater

Unleashed: [Unleashed Web Updater](https://lab.flipper.net/?url=https://unleashedflip.com/fw/unlshd-069/flipper-z-f7-update-unlshd-069.tgz&channel=release-cfw&version=unlshd-069)

Extreme: [Xtreme Web Updater](https://flipper-xtre.me/update/)

**Resources:**
> Below is further Resources that might help you if your struggling to install firmware on your Flipper

Ultimate Guide: [Talking Sasquatch : Starter Guide - YouTube](https://www.youtube.com/watch?v=12M_oHmxcCQ)

## Bad Bluetooth (BadKB)

I always tell others that Bad Bluetooth is basically a wireless rubber ducky attack. If you don't know what a rubber ducky is then that's ok. Bad KB and Rubber Duckies allows you to emulate a keyboard and send pre-programmed key strokes to any device you are connected to. In this case of a rubber ducky you must plug it into a computer as it looks like a "USB" device. But in this case The Flipper must connect over Bluetooth to the victims device to launch the attack.


## Bluetooth Impersonation Attacks

Our team recently gained recognition on TheHackerNews for uncovering a Bluetooth and macOS vulnerability. This exploit enabled us to mimic a Bluetooth device, such as AirPods or an iPhone, using a Flipper Zero. By impersonating the MAC Address, we could intercept or establish an initial connection with a victim's machine. The success of this approach relied on the machine itself trusting us as the legitimate AirPods or Bluetooth device of choice for the victim. Once the initial connection was established, our Flippers could then function as "HID Devices" (Keyboards) with the "Bad Bluetooth" (BadKB) module. This capability allowed us to send malicious keystrokes, ultimately granting us reverse shell access to the targeted machine.

**THN Article:**[ New BLUFFS Bluetooth Attack Expose Devices to Adversary-in-the-Middle Attacks (thehackernews.com)](https://thehackernews.com/2023/12/new-bluffs-bluetooth-attack-expose.html)

**Threat Locker Blog Post:**[ How Hackers Use Bluetooth to Take Over Your Mac Device | ThreatLocker](https://www.threatlocker.com/blog/mac-bluetooth-impersonation-attacks)

**Reverse Shell Payloads:**
* macOS:[ ZTW/Payload (github.com)](https://github.com/ThreatLockerIvan/ZTW24/blob/main/Flipper-Zero-Unleashed/Payloads/macos-reverseshell-payload.txt)
~~~
REM --- MACOS REVERSE SHELL ---
GUI r
~~~
* Windows: [ZTW/Payloads (github.com)](https://github.com/ThreatLockerIvan/ZTW24/blob/main/Flipper-Zero-Unleashed/Payloads/windows-revershell-payload.txc)
~~~
REM --- WINDOWS REVERSE SHELL ---
GUI r
~~~

##  NFC/RFID Hacking
The Flipper Zero is great for Hacking RFID and NFC cards or tags. To access these navigate to the "Apps" section on your device.

It has the following Capabilities:
* **Sniffing/Reading:**  The Flipper can read most cards/tags
* **Writing**  The Flipper can write to other cards/tags
* **Emulate** The Flipper can emulate a signal it has previously read and stored 

> Always be aware of the type of card or tag you are trying to hack with the Flipper, this helps you understand what module you need to use to actually be able to read it. For example, if you have an iClass card you don't want to be using the Mifare NFC Module.
