![ZTW Logo](../../Assets/Hacking_Labs_graphics_ztw_logo_med_1.png)

# Intro

Welcome to the `Hashcat` classes, in this class you will learn how to use
`Hashcat` to brute force password or using wordlist to do a wordlist attack.

- [Intro](#intro)
- [Overview of Hashcat](#overview-of-hashcat)
- [Identify Hashes](#identify-hashes)
- [How to use Hashcat](#how-to-use-hashcat)
- [Brute Force](#brute-force)
- [Word List](#word-list)
- [Practice time](#practice-time)
  - [Answers](#answers)
    - [1.](#1)
    - [2.](#2)
    - [3.](#3)
    - [4.](#4)
- [References](#references)

# Overview of Hashcat

Hashcat tool is a advanced password "recovery tool", if you know John the
ripper tool then you should an idea about types of password cracking. The
main difference between `hashcat` and John the ripper is that `Hashcat` can use
the GPU of a computer. And if you know anything about bitcoin mining, you know
that GPU is a lot better at that type of work load.

> Note: If you are using `hashcat` in a VM (virtual machine) you will have to
> bridge your GPU to the vm. Because in an VM `hashcat` will just use the CPU
> assigned to the VM.

The first thing to know about password cracking is how do we even get the hash
to crack. A hash refers to a cryptographic representation of a password or
other data, generated using a hash function. Hash functions are mathematical
algorithms that take an input (such as a password) and produce a fixed-size
string of characters.

Hash can look like any one of these,

```
MD5 - 8743b52063cd84097a65d1633f5c74f5
NetNTLMv2 - admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
SHA2-256 - 	127e6fbfe24a750e72930c220a8e138275656b8e5d8f48a98c3c92df2caba935
```

There is a lot more examples on the main Hashcat
site: https://hashcat.net/wiki/doku.php?id=example_hashes

# Identify Hashes

In the real world, if you don't know what type of hash something is you have to
make a guess on what hash it could be. Unless you know what type of hash it is
before hand, like in most windows computer would use the NetNTLMv2 or
NetNTLMv1. So what can you do if you have a hash and don't know what type of
hash it could be, then you have to use online or offline tool to make a guess
on what type of hash it is. We will be showing off the `hash-identifier` tool
that is install on most kali machines.

You can use `hash-identifier` in two ways.

- Option 1
  ```bash
    hash-identifier <hash>
    ```
- Option 2
  ``` bash
  hash-identifier
  ```
  Then insert the hash

You will get results like this:

![Hash-identifier results](../../Assets/OnDemand/Hashcat/hash_identifier.png)

> Note: you can use other tools, There is even ones on websites.

You can see that the most possible hash is MD5 or domain cached credentials -
MD4. So now we know that it is some type of MD4 or MD5 hash. There are many way
to crack this but we are going to so two ways.

- Brute force
- Word List

# How to use Hashcat

`Hashcat` can be very hard, there are many options you can pick from and you
might not be able to tell you add the wrong option till much later. The main
options that you need to know are the following:

- `-m` - This is how you tell Hashcat what hash to crack against. If this value
  is null, then Hashcat will start the autodetect for hash type.
- `-a` - This tells Hashcat how to crack the hashes you give it.
- `-h` - This will bring the help menu.
- `-o` - This is where the output will go, so it will go to the current
- directory you are in and put the results in out.txt
- `-p` - This is how you tell Hashcat what separator character to use in the
  hash lists and out file.
- `-b` - This will run a benchmark on the hashes.

If you want to know more options you can find them on the Hashcat site:
https://hashcat.net/wiki/doku.php?id=hashcat

# Brute Force

With brute forcing password we need to know a few things. In hash cat there are
things called Charsets which is just a set of character that can be use for one
spot of the password.

```
Charsets
?l # Lowercase a-z
?u # Uppercase A-Z
?d # Decimals
?h # Lowercase a-z with Decimals
?H # Uppercase A-Z with Decimals
?s # Special chars
?a # All (l,u,d,s)
?b # Binary
```

To better explain what Charset is, lets use it in an example.

```bash
sudo hashcat -m 0 -a 3 ?u?l?l?l?l?l?d -i hash.txt -o out.txt
```

- `sudo` - Hashcat need to run as admin level/ sudo level
- `m 0` - This is telling hashcat to crack for an md5 hash. if you are lost
- about find what hash are what go back to the hashcat site with all the
- example hashes.
- `-a 3` - This tells hashcat to use the brute force method.
- `?u?l?l?l?l?l?d` - This tells hashcat to look for a Uppercase letter for the
- 1st spot and look for 5 lower case letter for the next 5 spots and finally
- look for a number in the last spot.
- `-i` - Tells hashcat to increment as it goes, So it will start with just `?u`
- than add more.
- `-o` - This is where the output will go, so it will go to the current
- directory you are in and put the results in out.txt

So the following could be the password:

- Ztwaws1
- Qwerty1
- Summer2

You can also use no mask and `hashcat` will just start with `?a` and keep add
more `?a` till it crack the hash.

# Word List

With cracking with wordlist list is a lot easy to explain then brute forcing,
With word list you have a pregenerate list of password with the hash value map
to the password. This attack can be much faster than a brute force method.

```bash
sudo hashcat -m 100 -a 0 -o out.txt --force <Hash> /usr/share/wordlists/rockyou.txt
```
> Note: `Rockyou.txt` have to be unzip before use.
> `rockyou.txt` is store in `/usr/share/wordlists/` for kali.

- `sudo` - Hashcat need to run as admin level/ sudo level
- `m 100` - This is telling hashcat to crack for an sha1 hash. if you are lost
- about find what hash are what go back to the hashcat site with all the
- example hashes.
- `-a 0` - This tells hashcat to use the straight method(wordlist).
- `-o` - This is where the output will go, so it will go to the current
- directory you are in and put the results in out.txt
- `--force` - this will tell hashcat to ignore warnings.

# Practice time

Whats a better way to learn but to practice here are some hash that you can try:

```
1. - 482c811da5d5b4bc6d497ffa98491e38
2. - 35fddd95911c6e8ced55b83b4dddcb9a06a2af8471d412ad7427878369d3d18bb7ac00ec561a41d819596540d2edf600df56376c906f49ff5c93d04c0c42546d
3. - 9527ac750ae4efd88ff49b61b41e607eb365ae4bc2381c341330a37998f84ec3
4. - 6198B6DD3679B6FC2858A84BE31B8126
```

Good luck have fun (GLHF)

## Answers

If you got lost or you just want to see the answers, Threatlocker ops got you.

### 1.

This is an MD5 hash, you can always try a word list attack first and
then try a brute force attack.

This is the command to crack the MD5 hash with a word list:

``` bash
sudo hashcat -m 0 -a 0 482c811da5d5b4bc6d497ffa98491e38  /usr/share/wordlists/rockyou.txt
```

**Answer**

```
482c811da5d5b4bc6d497ffa98491e38 :password123
```

### 2.

This is a SHA512 hash
This is the command to crack the SHA512 with a word list:

```bash
sudo hashcat -m 1700 -a 0 35fddd95911c6e8ced55b83b4dddcb9a06a2af8471d412ad7427878369d3d18bb7ac00ec561a41d819596540d2edf600df56376c906f49ff5c93d04c0c42546d  /usr/share/wordlists/rockyou.txt
```

**Answer**

```
35fddd95911c6e8ced55b83b4dddcb9a06a2af8471d412ad7427878369d3d18bb7ac00ec561a41d819596540d2edf600df56376c906f49ff5c93d04c0c42546d  :password!
```

### 3.

This is a SHA256 hash
This is the command to crack the SHA256 with brute force.

```bash
sudo hashcat -m 1400 -a 3 9527ac750ae4efd88ff49b61b41e607eb365ae4bc2381c341330a37998f84ec3  ?a?a?a?a?a?a?a
```

> Note: we start with just one `?a` and then just kept adding more `?a`

**Answer**

```
9527ac750ae4efd88ff49b61b41e607eb365ae4bc2381c341330a37998f84ec3 :lilil17
```

### 4.

This is NTLM hash, This one you had to just go down the list of hash types

```bash
sudo hashcat -m 1000 -a 3 6198B6DD3679B6FC2858A84BE31B8126
```

> Note: This one might have took a while.

**Answer**

```
6198B6DD3679B6FC2858A84BE31B8126 :sa342jb
```

# References

- **Example Hashes:** https://hashcat.net/wiki/doku.php?id=example_hashes
- **Hashcat options:** https://hashcat.net/wiki/doku.php?id=hashcat
- **Hashcat Cheatsheet** https://cheatsheet.haax.fr/passcracking-hashfiles/hashcat_cheatsheet/