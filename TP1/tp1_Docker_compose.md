# III. Docker compose

🌞 **Créez un fichier `docker-compose.yml`**

```bash
[rocky@docker compose_test]$ cat docker-compose.yml 
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

🌞 **Lancez les deux conteneurs** avec `docker compose`

```bash
[rocky@docker compose_test]$ docker compose up -d
[+] Running 3/3
 ✔ Network compose_test_default                  Created                                                                                                                0.3s 
 ✔ Container compose_test-conteneur_flopesque-1  Started                                                                                                                0.1s 
 ✔ Container compose_test-conteneur_nul-1        Started
```

🌞 **Vérifier que les deux conteneurs tournent**

```bash
[rocky@docker compose_test]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED              STATUS              PORTS     NAMES
da5394e569ba   debian    "sleep 9999"   About a minute ago   Up About a minute             compose_test-conteneur_flopesque-1
273c5362dd1c   debian    "sleep 9999"   About a minute ago   Up About a minute             compose_test-conteneur_nul-1
```

🌞 **Pop un shell dans le conteneur `conteneur_nul`**

```bash
[rocky@docker compose_test]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED              STATUS              PORTS     NAMES
da5394e569ba   debian    "sleep 9999"   About a minute ago   Up About a minute             compose_test-conteneur_flopesque-1
273c5362dd1c   debian    "sleep 9999"   About a minute ago   Up About a minute             compose_test-conteneur_nul-1

[rocky@docker compose_test]$ docker exec -it 273c5362dd1c bash
root@273c5362dd1c:/# apt update
Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [52.1 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 Packages [8787 kB]
Get:5 http://deb.debian.org/debian bookworm-updates/main amd64 Packages [11.3 kB]
Get:6 http://deb.debian.org/debian-security bookworm-security/main amd64 Packages [128 kB]
Fetched 9177 kB in 2s (4777 kB/s)                         
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done

root@273c5362dd1c:/# apt install -y iputils-ping
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcap2-bin libpam-cap
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 96.2 kB of archives.
After this operation, 311 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main amd64 libcap2-bin amd64 1:2.66-4 [34.7 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 iputils-ping amd64 3:20221126-1 [47.1 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 libpam-cap amd64 1:2.66-4 [14.5 kB]
Fetched 96.2 kB in 0s (1028 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 6098 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.66-4_amd64.deb ...
Unpacking libcap2-bin (1:2.66-4) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20221126-1_amd64.deb ...
Unpacking iputils-ping (3:20221126-1) ...
Selecting previously unselected package libpam-cap:amd64.
Preparing to unpack .../libpam-cap_1%3a2.66-4_amd64.deb ...
Unpacking libpam-cap:amd64 (1:2.66-4) ...
Setting up libcap2-bin (1:2.66-4) ...
Setting up libpam-cap:amd64 (1:2.66-4) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.36.0 /us
r/local/share/perl/5.36.0 /usr/lib/x86_64-linux-gnu/perl5/5.36 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.36 /usr/share/perl/5.36 
/usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20221126-1) ...

root@273c5362dd1c:/# ping conteneur_flopesque -c 1
PING conteneur_flopesque (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.073 ms

--- conteneur_flopesque ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.073/0.073/0.073/0.000 ms
```
