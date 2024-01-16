# I. Init

- [I. Init](#i-init)
  - [2. Vérifier que Docker est bien là](#2-vérifier-que-docker-est-bien-là)
  - [3. sudo c pa bo](#3-sudo-c-pa-bo)
  - [4. Un premier conteneur en vif](#4-un-premier-conteneur-en-vif)
  - [5. Un deuxième conteneur en vif](#5-un-deuxième-conteneur-en-vif)

## 2. Vérifier que Docker est bien là

```bash
[rocky@docker ~]$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
     Active: active (running) since Thu 2023-12-21 10:58:13 CET; 11s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 5224 (dockerd)
      Tasks: 8
     Memory: 28.9M
        CPU: 191ms
     CGroup: /system.slice/docker.service
             └─5224 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Dec 21 10:58:12 docker systemd[1]: Starting Docker Application Container Engine...
Dec 21 10:58:12 docker dockerd[5224]: time="2023-12-21T10:58:12.373934965+01:00" level=info msg="Starting up"
Dec 21 10:58:12 docker dockerd[5224]: time="2023-12-21T10:58:12.448321497+01:00" level=info msg="Loading containers: start."
Dec 21 10:58:13 docker dockerd[5224]: time="2023-12-21T10:58:13.383798237+01:00" level=info msg="Firewalld: interface docker0 already part of docker zone, returning"
Dec 21 10:58:13 docker dockerd[5224]: time="2023-12-21T10:58:13.549034300+01:00" level=info msg="Loading containers: done."
Dec 21 10:58:13 docker dockerd[5224]: time="2023-12-21T10:58:13.569557003+01:00" level=info msg="Docker daemon" commit=311b9ff graphdriver=overlay2 version=24.0.7
Dec 21 10:58:13 docker dockerd[5224]: time="2023-12-21T10:58:13.569834678+01:00" level=info msg="Daemon has completed initialization"
Dec 21 10:58:13 docker dockerd[5224]: time="2023-12-21T10:58:13.631050021+01:00" level=info msg="API listen on /run/docker.sock"
Dec 21 10:58:13 docker systemd[1]: Started Docker Application Container Engine.
```

## 3. sudo c pa bo

🌞 **Ajouter votre utilisateur au groupe `docker`**

```bash
[rocky@docker ~]$ cat /etc/group | grep docker
docker:x:991:rocky

[rocky@docker ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 4. Un premier conteneur en vif

🌞 **Lancer un conteneur NGINX**

```bash
[rocky@docker ~]$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
af107e978371: Pull complete 
336ba1f05c3e: Pull complete 
8c37d2ff6efa: Pull complete 
51d6357098de: Pull complete 
782f1ecce57d: Pull complete 
5e99d351b073: Pull complete 
7b73345df136: Pull complete 
Digest: sha256:bd30b8d47b230de52431cc71c5cce149b8d5d4c87c204902acf2504435d4b4c9
Status: Downloaded newer image for nginx:latest
5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec
```

🌞 **Visitons**

```bash
[rocky@docker ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
5933921068b3   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:9999->80/tcp, :::9999->80/tcp   zen_franklin

[rocky@docker ~]$ docker logs 5933921068b3
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/12/21 10:05:22 [notice] 1#1: using the "epoll" event method
2023/12/21 10:05:22 [notice] 1#1: nginx/1.25.3
2023/12/21 10:05:22 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2023/12/21 10:05:22 [notice] 1#1: OS: Linux 5.14.0-284.30.1.el9_2.x86_64
2023/12/21 10:05:22 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2023/12/21 10:05:22 [notice] 1#1: start worker processes
2023/12/21 10:05:22 [notice] 1#1: start worker process 28

[rocky@docker ~]$ docker inspect 5933921068b3
[
    {
        "Id": "5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec",
        "Created": "2023-12-21T10:05:21.818994412Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 5581,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-12-21T10:05:22.465336228Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d453dd892d9357f3559b967478ae9cbc417b52de66b53142f6c16c8a275486b9",
        "ResolvConfPath": "/var/lib/docker/containers/5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec/hostname",
        "HostsPath": "/var/lib/docker/containers/5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec/hosts",
        "LogPath": "/var/lib/docker/containers/5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec/5933921068b3c126ee49c75d1aa40bae1b10cde7967c9a72556af11c025a29ec-json.log",
        "Name": "/zen_franklin",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "9999"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                40,
                173
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/7fd3a62a675c3410acd49fbb29cfb741bfab534544e8063faa5e42c9695f0a59-init/diff:/var/lib/docker/overlay2/1ccb98d422d487aba21a3146ca1311b6ebd8b365c2e568c0595ab8b944c0cd3e/diff:/var/lib/docker/overlay2/974c6c90a295e6ef4d14af01d5e466e54caaf574dba83e717b854cc3dcb024bf/diff:/var/lib/docker/overlay2/debf263207e285fcfca2e674eaf6808d8d18fa48b99aa96cdfd8e54e3d691e71/diff:/var/lib/docker/overlay2/1c3418b50e9fe12dc56083274c1485686fe324a8d40bae3fde1cf36b68f16edc/diff:/var/lib/docker/overlay2/e8e5afba04fb71262d7dc8aaffab770f02d223d055eedc481434bb3b2da7ef45/diff:/var/lib/docker/overlay2/523112fbd7822bc57360052f1bdefb1ff15cbb079f81b1207a505408739bdf14/diff:/var/lib/docker/overlay2/ffbd1ebdad97aa1fb7808f39fb202e221cd012555815222af1d3a3e11957075c/diff",
                "MergedDir": "/var/lib/docker/overlay2/7fd3a62a675c3410acd49fbb29cfb741bfab534544e8063faa5e42c9695f0a59/merged",
                "UpperDir": "/var/lib/docker/overlay2/7fd3a62a675c3410acd49fbb29cfb741bfab534544e8063faa5e42c9695f0a59/diff",
                "WorkDir": "/var/lib/docker/overlay2/7fd3a62a675c3410acd49fbb29cfb741bfab534544e8063faa5e42c9695f0a59/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "5933921068b3",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.25.3",
                "NJS_VERSION=0.8.2",
                "PKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "20a076eece59417e577eb6bf64e5fede3f8a04267168c0f131b1b171a5197488",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "9999"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "9999"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/20a076eece59",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "1a6bdf439213606a5b3a308bc5f51f443c18ba350e1e2a4502c153687378c335",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "686f5fbcfbad2fbe8e172536757cbdc91a3c154f8db654761eadbdb8fb142eb6",
                    "EndpointID": "1a6bdf439213606a5b3a308bc5f51f443c18ba350e1e2a4502c153687378c335",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

[rocky@docker ~]$ sudo ss -lnpt | grep docker
LISTEN 0      4096         0.0.0.0:9999      0.0.0.0:*    users:(("docker-proxy",pid=5538,fd=4))
LISTEN 0      4096            [::]:9999         [::]:*    users:(("docker-proxy",pid=5543,fd=4))

[rocky@docker ~]$ sudo firewall-cmd --add-port=9999/tcp
success

[rocky@docker ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 9999/tcp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

```bash
geoffrey > curl 192.168.56.7:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

🌞 **On va ajouter un site Web au conteneur NGINX**

```bash
[rocky@docker nginx]$ cat index.html 
<h1>ratio</h1>

[rocky@docker nginx]$ cat site_nul.conf 
server {
    listen        8080;

    location / {
        root /var/www/html/;
    }
}

[rocky@docker nginx]$ docker run -d -p 9999:8080 -v /home/rocky/nginx/index.html:/var/www/html/index.html -v /home/rocky/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
8a04afca4cacb41875dbbbcd9f6f9bbf4eaa8e76b043bcdae846e3a0c295c9e8

[rocky@docker nginx]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                               NAMES
8a04afca4cac   nginx     "/docker-entrypoint.…"   3 seconds ago   Up 2 seconds   80/tcp, 0.0.0.0:9999->8080/tcp, :::9999->8080/tcp   jovial_wescoff
```

🌞 **Visitons**

```bash
geoffrey > curl 192.168.56.7:9999
<h1>ratio</h1>
```

## 5. Un deuxième conteneur en vif

🌞 **Lance un conteneur Python, avec un shell**

```bash
[rocky@docker nginx]$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
bc0734b949dc: Pull complete 
b5de22c0f5cd: Pull complete 
917ee5330e73: Pull complete 
b43bd898d5fb: Pull complete 
7fad4bffde24: Pull complete 
d685eb68699f: Pull complete 
107007f161d0: Pull complete 
02b85463d724: Pull complete 
Digest: sha256:3733015cdd1bd7d9a0b9fe21a925b608de82131aa4f3d397e465a1fcb545d36f
Status: Downloaded newer image for python:latest
root@e7b526798ff9:/# docker ps
bash: docker: command not found
root@e7b526798ff9:/# python3
Python 3.12.1 (main, Dec 19 2023, 20:14:15) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("ratio")
ratio
>>> exit()
```

🌞 **Installe des libs Python**

```bash
root@e7b526798ff9:/# pip install aiohttp aioconsole
Requirement already satisfied: aiohttp in /usr/local/lib/python3.12/site-packages (3.9.1)
Requirement already satisfied: aioconsole in /usr/local/lib/python3.12/site-packages (0.7.0)
Requirement already satisfied: attrs>=17.3.0 in /usr/local/lib/python3.12/site-packages (from aiohttp) (23.1.0)
Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/lib/python3.12/site-packages (from aiohttp) (6.0.4)
Requirement already satisfied: yarl<2.0,>=1.0 in /usr/local/lib/python3.12/site-packages (from aiohttp) (1.9.4)
Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/lib/python3.12/site-packages (from aiohttp) (1.4.1)
Requirement already satisfied: aiosignal>=1.1.2 in /usr/local/lib/python3.12/site-packages (from aiohttp) (1.3.1)
Requirement already satisfied: idna>=2.0 in /usr/local/lib/python3.12/site-packages (from yarl<2.0,>=1.0->aiohttp) (3.6)
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

[notice] A new release of pip is available: 23.2.1 -> 23.3.2
[notice] To update, run: pip install --upgrade pip

root@e7b526798ff9:/# python3
Python 3.12.1 (main, Dec 19 2023, 20:14:15) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
>>> exit()
```
