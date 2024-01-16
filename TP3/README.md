# TP3 : Linux Hardening

## Sommaire

- [TP3 : Linux Hardening](#tp3--linux-hardening)
  - [Sommaire](#sommaire)
  - [1. Guides CIS](#1-guides-cis)
  - [2. Conf SSH](#2-conf-ssh)
  - [4. DoT](#4-dot)
  - [5. AIDE](#5-aide)

## 1. Guides CIS

🌞 **Suivre un guide CIS**

### Section 2.1 : Time Synchronization

```bash
[rocky@jeSuisSecuTKT ~]$ rpm -q chrony
chrony-4.3-1.el9.x86_64

[rocky@jeSuisSecuTKT ~]$ grep -E "^(server|pool)" /etc/chrony.conf
pool 2.rocky.pool.ntp.org iburst

[rocky@jeSuisSecuTKT ~]$ grep ^OPTIONS /etc/sysconfig/chronyd
OPTIONS="-F 2"

[rocky@jeSuisSecuTKT ~]$ grep ^OPTIONS /etc/sysconfig/chronyd
OPTIONS="-F 2 -u chrony"
```

### Section 3.1 : Disable unused network protocols and devices

```bash
[rocky@jeSuisSecuTKT ~]$ grep -Pqs '^\h*0\b' /sys/module/ipv6/parameters/disable && echo -e "\n -
IPv6 is enabled\n" || echo -e "\n - IPv6 is not enabled\n"

 -
IPv6 is enabled

[rocky@jeSuisSecuTKT ~]$ ./wireless.sh 

- Audit Result:
 ** PASS **

 - System has no wireless NICs installed

[rocky@jeSuisSecuTKT ~]$ ./tipc.sh

- Audit Result:
 ** PASS **

 - Module "tipc" doesn't exist on the
system                              
```

### Section 3.2 : Network Parameters (Host Only)

```bash
[rocky@jeSuisSecuTKT ~]$ cat network_params.sh 
# No IP forwarding
printf "
net.ipv4.ip_forward = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.ip_forward=0
printf "
net.ipv6.conf.all.forwarding = 0
" >> /etc/sysctl.d/60-netipv6_sysctl.conf
sysctl -w net.ipv6.conf.all.forwarding=0

# No packet redirect
printf "
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.send_redirects=0
sysctl -w net.ipv4.conf.default.send_redirects=0

sysctl -w net.ipv4.route.flush=1
[rocky@jeSuisSecuTKT ~]$ chmod +x network_params.sh 
[rocky@jeSuisSecuTKT ~]$ sudo ./network_params.sh 
net.ipv4.ip_forward = 0
net.ipv6.conf.all.forwarding = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.route.flush = 1
```

### Section 3.3 : Network Parameters (Host and Router)

```bash
[rocky@jeSuisSecuTKT ~]$ cat network_params2.sh 
# No routed packets
printf "
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.accept_source_route=0
sysctl -w net.ipv4.conf.default.accept_source_route=0
printf "
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
" >> /etc/sysctl.d/60-netipv6_sysctl.conf
sysctl -w net.ipv6.conf.all.accept_source_route=0
sysctl -w net.ipv6.conf.default.accept_source_route=0

# No ICMP redirects
printf "
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.accept_redirects=0
sysctl -w net.ipv4.conf.default.accept_redirects=0
printf "
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
" >> /etc/sysctl.d/60-netipv6_sysctl.conf
sysctl -w net.ipv6.conf.all.accept_redirects=0
sysctl -w net.ipv6.conf.default.accept_redirects=0

# No secure ICMP redirect
printf "
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.secure_redirects=0
sysctl -w net.ipv4.conf.default.secure_redirects=0

# Logging suspicious packets
printf "
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.log_martians=1
sysctl -w net.ipv4.conf.default.log_martians=1

# Ignoring broadcast ICMP requests
printf "
net.ipv4.icmp_echo_ignore_broadcasts = 1
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.icmp_echo_ignore_broadcasts=1

# Ignoring bogus ICMP responses
printf "
net.ipv4.icmp_ignore_bogus_error_responses = 1
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.icmp_ignore_bogus_error_responses=1

# Enabling reverse path filtering
printf "
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.conf.all.rp_filter=1
sysctl -w net.ipv4.conf.default.rp_filter=1

# Enable TCP SYN cookies
printf "
net.ipv4.tcp_syncookies = 1
" >> /etc/sysctl.d/60-netipv4_sysctl.conf
sysctl -w net.ipv4.tcp_syncookies=1

# Ignoring IPv6 routeur advertisements
printf "
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.default.accept_ra = 0
" >> /etc/sysctl.d/60-netipv6_sysctl.conf
sysctl -w net.ipv6.conf.all.accept_ra=0
sysctl -w net.ipv6.conf.default.accept_ra=0

sysctl -w net.ipv6.route.flush=1

[rocky@jeSuisSecuTKT ~]$ sudo ./network_params2.sh 
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.tcp_syncookies = 1
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.default.accept_ra = 0
net.ipv6.route.flush = 1
```

### 5.2 : Configure SSH Server

Vérification des autorisations sur les fichiers de configurations et clés SSH :

```bash
[rocky@jeSuisSecuTKT ~]$ ls -l /etc/ssh/sshd_config
-rw------- 1 root root 3667 Nov  2 20:33 /etc/ssh/sshd_config

[rocky@jeSuisSecuTKT ~]$ find /etc/ssh/ -iname *key* -exec ls -la {} ';' 2> /dev/null 
-rw-r-----. 1 root ssh_keys 492 Oct 24 10:35 /etc/ssh/ssh_host_ecdsa_key
-rw-r--r--. 1 root root 162 Oct 24 10:35 /etc/ssh/ssh_host_ecdsa_key.pub
-rw-r-----. 1 root ssh_keys 387 Oct 24 10:35 /etc/ssh/ssh_host_ed25519_key
-rw-r--r--. 1 root root 82 Oct 24 10:35 /etc/ssh/ssh_host_ed25519_key.pub
-rw-r-----. 1 root ssh_keys 2578 Oct 24 10:35 /etc/ssh/ssh_host_rsa_key
-rw-r--r--. 1 root root 554 Oct 24 10:35 /etc/ssh/ssh_host_rsa_key.pub
```

Limiter l'accès ssh à un user :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'AllowUsers'
AllowUsers rocky
```

Activer les logs :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'LogLevel'
LogLevel VERBOSE
```

Activer UsePAM :

```
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'UsePAM'
# WARNING: 'UsePAM no' is not supported in RHEL and may cause several
UsePAM yes
```

Désactiver root login :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'PermitRootLogin'
PermitRootLogin no
```

Désactiver les HostBasedAuthentification :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'HostbasedAuthentication'
HostbasedAuthentication no
```

Désactiver les mots de passes vides :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'PermitEmpty'
PermitEmptyPasswords no
```

Désactiver l'accès aux variables d'environnements d'sshd pour les users :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'PermitUser'
PermitUserEnvironment no
```

Forcer les utilisateurs à utiliser un mot de passe pour se connecter plutôt que des fichers rhosts :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'IgnoreRhos'
IgnoreRhosts yes
```

Désactiver le X11Forwarding :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'X11Forwarding'
X11Forwarding no
```

Désactiver le TcpForwading pour empêcher un utilisateur malveillant de chiffer son activité :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'AllowTcpForwarding'
AllowTcpForwarding no
```

Vérifier que le système n'override pas le chiffrement d'openSSH :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo grep -i '^\s*CRYPTO_POLICY=' /etc/sysconfig/sshd
```

Active une bannière d'avertissement avant connexion d'un utilisateur :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'Banner'
Banner /etc/issue.net
``` 

Aujouter un nombre limité de tentatives de connxions :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'MaxAuth'
MaxAuthTries 4
```

Limiter le nombre de connexions non authentifiés en simultanés authorisées :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'MaxStar'
MaxStartups 10:30:100
```

Limiter le nombre de sessions ouverte en simultanées depuis une connexion :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'MaxSess'
MaxSessions 10
```

Limiter le temps authorisé pour des connexions non authentifiées :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'LoginG'
LoginGraceTime 60
```

Définir l'interval de temps à partir duquel le client est considérer inactif et déconnecté :

```bash
[rocky@jeSuisSecuTKT ~]$ sudo cat /etc/ssh/sshd_config | grep 'ClientA'
ClientAliveInterval 15
ClientAliveCountMax 3
```

### 6.1 : System File Permissions

Permissions restreintes sur /etc/passwd, /etc/passwd-, /etc/group, /etc/shadow, /etc/shadow-, /etc/gshadow, /etc/gshadow- :

```console
[rocky@jeSuisSecuTKT ~]$ ls -la /etc/passwd
-rw-r--r--. 1 root root 1037 Oct 24 10:41 /etc/passwd

[rocky@jeSuisSecuTKT ~]$ ls -la /etc/passwd- 
-rw-r--r--. 1 root root 1004 Oct 24 10:36 /etc/passwd-

[rocky@jeSuisSecuTKT ~]$ ls -la /etc/group
-rw-r--r--. 1 root root 511 Oct 24 10:41 /etc/group

[rocky@jeSuisSecuTKT ~]$ ls -la /etc/group-
-rw-r--r--. 1 root root 497 Oct 24 10:36 /etc/group-

[rocky@jeSuisSecuTKT ~]$ ls -l /etc/shadow
----------. 1 root root 761 Oct 24 10:41 /etc/shadow

[rocky@jeSuisSecuTKT ~]$ ls -l /etc/shadow-
----------. 1 root root 738 Oct 24 10:36 /etc/shadow-

[rocky@jeSuisSecuTKT ~]$ ls -l /etc/gshadow
----------. 1 root root 402 Oct 24 10:41 /etc/gshadow

[rocky@jeSuisSecuTKT ~]$ ls -l /etc/gshadow-
----------. 1 root root 390 Oct 24 10:36 /etc/gshadow-
```

Verifier qu'il n'existe pas de fichiers écrivables par tous :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type f -perm -0002 2> /dev/null
```

Vérifier qu'il n'existe pas de dossier ou fichier sans propriétaire :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk {'if (NR!=1) print $6'} | xargs -I '{}' find '{}' -xdev -nouser 2> /dev/null
```

Vérifier qu'il n'existe pas de dossier ou fichier sans groupe :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -nogroup 2> /dev/null
```

Vérifier qu'il n'existe pas de dossier écrivable par tous sans le sticky bit :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type d \( -perm -0002 -a ! -perm -1000 \) 2>/dev/null
```

Vérifier que les programmes SUID sont légitimes :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type f -perm -4000 2> /dev/null 
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/su
/usr/bin/mount
/usr/bin/umount
/usr/bin/crontab
/usr/bin/passwd
/usr/bin/sudo
/usr/sbin/unix_chkpwd
/usr/sbin/pam_timestamp_check
/usr/sbin/grub2-set-bootflag
```

Vérifier que les programmes SGID sont légitimes :

```console
[rocky@jeSuisSecuTKT /]$ df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type f -perm -2000 2> /dev/null
/usr/bin/write
/usr/libexec/utempter/utempter
/usr/libexec/openssh/ssh-keysign
```

### 5.3 : Configure privilege escalation

Vérifier que sudo est installé :

```console
[rocky@jeSuisSecuTKT /]$ dnf list sudo
Last metadata expiration check: 0:00:04 ago on Tue 16 Jan 2024 04:12:02 PM CET.
Installed Packages
sudo.x86_64                                           1.9.5p2-9.el9                                           @minimal
```

Empêcher de démarrer lancer un daemon malveillant avec sudo :

```console
[rocky@jeSuisSecuTKT /]$ sudo cat /etc/sudoers | grep 'use_pty'
Defaults use_pty
```



## 2. Conf SSH

![SSH](./img/ssh.jpg)

🌞 **Chiffrement fort côté serveur**

- trouver une ressource de confiance (je veux le lien en compte-rendu)
- configurer le serveur SSH pour qu'il utilise des paramètres forts en terme de chiffrement (je veux le fichier de conf dans le compte-rendu)
  - conf dans le fichier de conf
  - regénérer des clés pour le serveur ?
  - regénérer les paramètres Diffie-Hellman ? (se renseigner sur Diffie-Hellman ?)

🌞 **Clés de chiffrement fortes pour le client**

- trouver une ressource de confiance (je veux le lien en compte-rendu)
- générez-vous une paire de clés qui utilise un chiffrement fort et une passphrase
- ne soyez pas non plus absurdes dans le choix du chiffrement quand je dis "fort" (genre pas de RSA avec une clé de taile 98789080932083209 bytes)

🌞 **Connectez-vous en SSH à votre VM avec cette paire de clés**

- prouvez en ajoutant `-vvvv` sur la commande `ssh` de connexion que vous utilisez bien cette clé là

## 4. DoT

Ca commence à faire quelques années maintenant que plusieurs acteurs poussent pour qu'on fasse du DNS chiffré, et qu'on arrête d'envoyer des requêtes DNS en clair dans tous les sens.

Le Dot est une techno qui va dans ce sens : DoT pour DNS over TLS. On fait nos requêtes DNS dans des tunnels chiffrés avec le protocole TLS.

🌞 **Configurer la machine pour qu'elle fasse du DoT**

- installez `systemd-networkd` sur la machine pour ça
- activez aussi DNSSEC tant qu'on y est
- référez-vous à cette doc qui est cool par exemple
- utilisez le serveur public de CloudFlare : 1.1.1.1 (il supporte le DoT)

🌞 **Prouvez que les requêtes DNS effectuées par la machine...**

- ont une réponse qui provient du serveur que vous avez conf (normalement c'est `127.0.0.1` avec `systemd-networkd` qui tourne)
  - quand on fait un `dig ynov.com` on voit en bas quel serveur a répondu
- mais qu'en réalité, la requête a été forward vers 1.1.1.1 avec du TLS
  - je veux une capture Wireshark à l'appui !

## 5. AIDE

Un truc demandé au point 1.3.1 du guide CIS c'est d'installer AIDE.

AIDE est un IDS ou *Intrusion Detection System*. Les IDS c'est un type de programme dont les fonctions peuvent être multiples.

Dans notre cas, AIDE, il surveille que certains fichiers du disque n'ont pas été modifiés. Des fichiers comme `/etc/shadow` par exemple.

🌞 **Installer et configurer AIDE**

- et bah incroyable mais [une très bonne ressource ici](https://www.it-connect.fr/aide-utilisation-et-configuration-dune-solution-de-controle-dintegrite-sous-linux/)
- configurez AIDE pour qu'il surveille (fichier de conf en compte-rendu)
  - le fichier de conf du serveur SSH
  - le fichier de conf du client chrony (le service qui gère le temps)
  - le fichier de conf de `systemd-networkd`

🌞 **Scénario de modification**

- introduisez une modification dans le fichier de conf du serveur SSH
- montrez que AIDE peut la détecter

🌞 **Timer et service systemd**

- créez un service systemd qui exécute un check AIDE
  - il faut créer un fichier `.service` dans le dossier `/etc/systemd/system/`
  - contenu du fichier à montrer dans le compte rendu
- créez un timer systemd qui exécute un check AIDE toutes les 10 minutes
  - il faut créer un fichier `.timer` dans le dossier `/etc/systemd/system/`
  - il doit porter le même nom que le service, genre `aide.service` et `aide.timer`
  - c'est complètement irréaliste 10 minutes, mais ça vous permettra de faire des tests (vous pouvez même raccourcir encore)

