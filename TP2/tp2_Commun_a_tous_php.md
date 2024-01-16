# TP2 Commun : Stack PHP

## Sommaire

- [TP2 Commun : Stack PHP](#tp2-commun--stack-php)
  - [Sommaire](#sommaire)
- [I. Packaging de l'app PHP](#i-packaging-de-lapp-php)

# I. Packaging de l'app PHP

J'vous oriente dans la démarche :

➜ **on a dit qu'on voulait 3 conteneurs**

- parce qu'on est pas des animaux à tout mettre dans le même
- un conteneur = un process please

➜ **d'abord on prend des infos sur les images dispos**

- [PHP y'a une image officielle](https://hub.docker.com/_/php), lisez le README pour voir comment s'en servir
  - on dirait que le plus simple c'est de faire votre propre Dockerfile
  - surtout si vous avez besoin d'ajouter des libs
  - à vous de voir, lisez attentivement le README
- [idem pour MySQL](https://hub.docker.com/_/mysql)
  - là pas besoin de Dockerfile, on utilise direct l'image
  - on peut config :
    - un user et son password, ainsi qu'une database à créer au lancement du conteneur
    - direct via des variables d'environnement
    - c'est de ouf pratique
  - on peut aussi jeter un fichier `.sql` dans le bon dossier (lire le README) avec un volume, et il sera exécuté au lancement
    - parfait pour créer un schéma de base
- par contre pour PHPMyAdmin
  - pas d'image officielle
  - cherchez sur le Docker Hub y'a plusieurs gars qui l'ont packagé
  - c'est très répandu, donc y'a forcément une image qui fonctionne bien

➜ **ensuite on run les bails**

- vous pouvez jouer avec des `docker run` un peu pour utiliser les images et voir comment elles fonctionnent
- rapidement passer à la rédaction d'un `docker-compose.yml` qui lance les trois
- lisez bien les README de vos images, y'a tout ce qu'il faut

Pour ce qui est du contenu du `docker-compose.yml`, à priori :

- **il déclare 3 conteneurs**
  - **PHP + Apache**
    - un volume qui place votre code PHP dans le conteneur
    - partage de port pour accéder à votre site
  - **MySQL**
    - définition d'un user, son mot de passe, un nom de database à créer avec des variables d'environnement
    - injection d'un fichier `.sql`
      - pour créer votre schéma de base au lancement du conteneur
      - injecter des données simulées je suppose ?
  - **PHPMyAdmin**
    - dépend de l'image que vous utilisez
    - partage de port pour accéder à l'interface de PHPMyAdmin
- en fin de TP1, vous avez vu que vous pouviez `ping <NOM_CONTENEUR>`
  - **donc dans ton code PHP, faut changer l'IP de la base de données à laquelle tu te co**
  - ça doit être vers le nom du conteneur de base de données

> *Donc : dès qu'un conteneur est déclaré dans un `docker-compose.yml` il peut joindre tous les autres via leurs noms sur le réseau. Et c'est bien pratique. **Nik les adresses IPs.***

Bon j'arrête de blabla, voilà le soleil.

🌞 **`docker-compose.yml`**

- genre `tp2/php/docker-compose.yml` dans votre dépôt git de rendu
- votre code doit être à côté dans un dossier `src` : `tp2/php/src/tous_tes_bails.php`
- s'il y a un script SQL qui est injecté dans la base à son démarrage, il doit être dans `tp2/php/sql/seed.sql`
  - on appelle ça "seed" une database quand on injecte le schéma de base et éventuellement des données de test
- bah juste voilà ça doit fonctionner : je git clone ton truc, je `docker compose up` et ça doit fonctionne :)
- ce serait cool que l'app affiche un truc genre `App is ready on http://localhost:80` truc du genre dans les logs !

➜ **Un environnement de dév local propre avec Docker**

- 3 conteneurs, donc environnement éphémère/destructible
- juste un **`docker-compose.yml`** donc facilement transportable
- TRES facile de mettre à jour chacun des composants si besoin
  - oh tiens il faut ajouter une lib !
  - oh tiens il faut une autre version de PHP !
  - tout ça c'est np

![save urself](img/save_urself.png)

