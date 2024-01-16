# TP2 Commun : Stack PHP

```shell
geoffrey > docker compose up
[+] Running 4/4
 ✔ Network commun_default         Created                                                                        0.2s 
 ✔ Container commun-db-1          Created                                                                        0.1s 
 ✔ Container commun-phpmyadmin-1  Created                                                                        0.1s 
 ✔ Container commun-php-1         Created                                                                        0.1s 
Attaching to commun-db-1, commun-php-1, commun-phpmyadmin-1
commun-db-1          | 2024-01-16 09:42:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.2.0-1.el8 started.
commun-phpmyadmin-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.20.0.4. Set the 'ServerName' directive globally to suppress this message
commun-phpmyadmin-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.20.0.4. Set the 'ServerName' directive globally to suppress this message
commun-phpmyadmin-1  | [Tue Jan 16 09:42:23.377592 2024] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.57 (Debian) PHP/8.2.13 configured -- resuming normal operations
commun-phpmyadmin-1  | [Tue Jan 16 09:42:23.377650 2024] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
commun-db-1          | 2024-01-16 09:42:23+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
commun-db-1          | 2024-01-16 09:42:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.2.0-1.el8 started.
commun-db-1          | 2024-01-16 09:42:23+00:00 [Note] [Entrypoint]: Initializing database files
commun-db-1          | 2024-01-16T09:42:23.712552Z 0 [System] [MY-015017] [Server] MySQL Server Initialization - start.
commun-db-1          | 2024-01-16T09:42:23.715178Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
commun-db-1          | 2024-01-16T09:42:23.715356Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.2.0) initializing of server in progress as process 81
commun-db-1          | 2024-01-16T09:42:23.726338Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
commun-db-1          | 2024-01-16T09:42:24.117042Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
commun-db-1          | 2024-01-16T09:42:25.669622Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
commun-db-1          | 2024-01-16T09:42:28.683005Z 0 [System] [MY-015018] [Server] MySQL Server Initialization - end.
commun-db-1          | 2024-01-16 09:42:28+00:00 [Note] [Entrypoint]: Database files initialized
commun-db-1          | 2024-01-16 09:42:28+00:00 [Note] [Entrypoint]: Starting temporary server
commun-db-1          | 2024-01-16T09:42:28.772332Z 0 [System] [MY-015015] [Server] MySQL Server - start.
commun-db-1          | 2024-01-16T09:42:28.987289Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
commun-db-1          | 2024-01-16T09:42:28.988445Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.2.0) starting as process 125
commun-db-1          | 2024-01-16T09:42:29.006433Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
commun-db-1          | 2024-01-16T09:42:29.149911Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
commun-db-1          | 2024-01-16T09:42:29.443007Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
commun-db-1          | 2024-01-16T09:42:29.443037Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
commun-db-1          | 2024-01-16T09:42:29.445324Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
commun-db-1          | 2024-01-16T09:42:29.468523Z 7 [ERROR] [MY-000061] [Server] 1046  No database selected.
commun-db-1          | 2024-01-16T09:42:29.469590Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.2.0'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server - GPL.
commun-db-1          | 2024-01-16T09:42:29.469577Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: /var/run/mysqld/mysqlx.sock
commun-db-1          | 2024-01-16T09:42:29.471108Z 0 [System] [MY-015016] [Server] MySQL Server - end.
commun-db-1          | 2024-01-16 09:42:29+00:00 [Note] [Entrypoint]: Temporary server started.
commun-db-1          | '/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/leapseconds' as time zone. Skipping it.
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/tzdata.zi' as time zone. Skipping it.
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
commun-db-1          | Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
commun-db-1          | 2024-01-16 09:42:31+00:00 [Note] [Entrypoint]: Creating database hello
commun-db-1          | 
commun-db-1          | 2024-01-16 09:42:31+00:00 [Note] [Entrypoint]: Stopping temporary server
commun-db-1          | 2024-01-16T09:42:31.564946Z 12 [System] [MY-013172] [Server] Received SHUTDOWN from user root. Shutting down mysqld (Version: 8.2.0).
commun-db-1          | 2024-01-16T09:42:33.194041Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.2.0)  MySQL Community Server - GPL.
commun-db-1          | 2024-01-16T09:42:33.198057Z 0 [System] [MY-015016] [Server] MySQL Server - end.
commun-db-1          | 2024-01-16 09:42:33+00:00 [Note] [Entrypoint]: Temporary server stopped
commun-db-1          | 
commun-db-1          | 2024-01-16 09:42:33+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.
commun-db-1          | 
commun-db-1          | 2024-01-16T09:42:33.591094Z 0 [System] [MY-015015] [Server] MySQL Server - start.
commun-db-1          | 2024-01-16T09:42:33.822716Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
commun-db-1          | 2024-01-16T09:42:33.824338Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.2.0) starting as process 1
commun-db-1          | 2024-01-16T09:42:33.835539Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
commun-db-1          | 2024-01-16T09:42:33.985811Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
commun-db-1          | 2024-01-16T09:42:34.242341Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
commun-db-1          | 2024-01-16T09:42:34.242377Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
commun-db-1          | 2024-01-16T09:42:34.245490Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
commun-db-1          | 2024-01-16T09:42:34.271662Z 7 [ERROR] [MY-000061] [Server] 1046  No database selected.
commun-db-1          | 2024-01-16T09:42:34.273155Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
commun-db-1          | 2024-01-16T09:42:34.273225Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.2.0'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
```

```console
geoffrey > docker ps
CONTAINER ID   IMAGE        COMMAND                  CREATED              STATUS              PORTS                                   NAMES
0e7cd24c4a4a   php:ratio    "docker-php-entrypoi…"   About a minute ago   Up About a minute                                           commun-php-1
564b4aa00821   phpmyadmin   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, :::8080->80/tcp   commun-phpmyadmin-1
ee0032d95c95   mysql        "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp, 33060/tcp                     commun-db-1
```
