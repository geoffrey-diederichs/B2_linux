# II. Images

- [II. Images](#ii-images)
  - [1. Images publiques](#1-images-publiques)
  - [2. Construire une image](#2-construire-une-image)

## 1. Images publiques

🌞 **Récupérez des images**

```bash
[rocky@docker nginx]$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED       SIZE
mysql                latest    73246731c4b0   2 days ago    619MB
linuxserver/wikijs   latest    869729f6d3c5   5 days ago    441MB
python               latest    fc7a60e86bae   13 days ago   1.02GB
wordpress            latest    fd2f5a0c6fba   2 weeks ago   739MB
python               3.11      22140cbb3b0c   2 weeks ago   1.01GB
nginx                latest    d453dd892d93   8 weeks ago   187MB
```

🌞 **Lancez un conteneur à partir de l'image Python**

```bash
[rocky@docker nginx]$ docker run -it python:3.11 bash
root@3ef6cc7328f3:/# python --version
Python 3.11.7
```

## 2. Construire une image

🌞 **Ecrire un Dockerfile pour une image qui héberge une application Python**

```bash
[rocky@docker python_app_build]$ cat app.py 
import emoji

print(emoji.emojize("Cet exemple d'application est vraiment naze :thumbs_down:"))

[rocky@docker python_app_build]$ cat Dockerfile 
FROM debian

RUN apt update && apt install -y python3 && apt install -y python3-emoji && mkdir app

WORKDIR /app

COPY app.py /app/app.py

ENTRYPOINT ["python3", "app.py"]
```

🌞 **Build l'image**

```bash
[rocky@docker python_app_build]$ docker build . -t coucou
[+] Building 16.8s (9/9) FINISHED                                                                                                                             docker:default
 => [internal] load build definition from Dockerfile                                                                                                                    0.0s
 => => transferring dockerfile: 211B                                                                                                                                    0.0s
 => [internal] load .dockerignore                                                                                                                                       0.0s
 => => transferring context: 2B                                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/debian:latest                                                                                                        0.0s
 => CACHED [1/4] FROM docker.io/library/debian                                                                                                                          0.0s
 => [internal] load build context                                                                                                                                       0.0s
 => => transferring context: 27B                                                                                                                                        0.0s
 => [2/4] RUN apt update && apt install -y python3 && apt install -y python3-emoji && mkdir app                                                                        15.9s
 => [3/4] WORKDIR /app                                                                                                                                                  0.1s
 => [4/4] COPY app.py /app/app.py                                                                                                                                       0.0s
 => exporting to image                                                                                                                                                  0.7s 
 => => exporting layers                                                                                                                                                 0.7s 
 => => writing image sha256:a6f69a4c6bfd33db48791a5396544095c97cc1ea332ec667d9eef9e17b058f61                                                                            0.0s 
 => => naming to docker.io/library/coucou
```

🌞 **Lancer l'image**

- lance l'image avec `docker run` :

```bash
[rocky@docker python_app_build]$ docker images | grep coucou
coucou               latest    a6f69a4c6bfd   21 seconds ago   189MB

[rocky@docker python_app_build]$ docker run coucou
Cet exemple d'application est vraiment naze 👎
```
