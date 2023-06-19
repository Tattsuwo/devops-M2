Basic :

j'ai créé 3 répertoires pour les différents images qu'on va vouloir créer : http serveur, backend API et BDD
en mettant le Dockerfile dans chacun des dossiers pour générer l'image.

création de l'image pour la bdd posgres et lancement de l'image :

docker build -t tatsuwo/my_postgres .

docker run -d -p 8080:5432 --net=app-network --name my_postgres tatsuwo/my_postgres
docker network create app-network


ajout des lignes : 
COPY CreateScheme.sql /docker-entrypoint-initdb.d
COPY InsertData.sql /docker-entrypoint-initdb.d

pour copier et utiliser le script sur la sortie /docker-entrypoint-initdb.d


lancement de la BDD : 
docker run -p 8090:8080 --net=app-network --name=adminer -d adminer


retirer une image : 
docker rm -f my_postgres



Enlever la persistance des données :
docker run -d -p 8080:5432 --net=app-network -v /my/own/datadir:/var/lib/postgresql/data --name my_postgres tatsuwo/my_postgres





serveur : my_postgres
utilisateur : usr
mdp : pwd
bdd : db



-------------------------------------

Dockerfile : 
FROM openjdk

COPY Main.class /

ENTRYPOINT ["java"] 

CMD ["Main"]


docker build -t tatsuwo/myjava .

Create your Springboot application :
https://start.spring.io/

Use the following config:

    Project: Maven
    Language: Java 17
    Spring Boot: 2.7.5
    Packaging: Jar
    Dependencies: Spring Web


ajout du code dans GreetingController.java
docker build -t tatsuwo/myappli .
docker run -d -p 8080:8080 tatsuwo/myappli --name myappli tatsuwo/myappli




1-2 Why do we need a multistage build? And explain each step of this dockerfile
FAIRE LA QUESTION PLUS TARD




Je met les config de la BDD dans le fichier yml


docker build -t tatsuwo/mynewappli .

docker run -d -p 8070:5432 --net=app-network --name my_postgres tatsuwo/my_postgres
docker run -p 8090:8080 --net=app-network --name=adminer -d adminer
docker run -d -p 8060:8080 --net=app-network --name mynewappli tatsuwo/mynewappli

http://localhost:8060/
---------------------------------------
HTTP SERVER

docker build -t tatsuwo/my_http_server .
docker run -d -p 8080:80 --net=app-network --name my_http_server tatsuwo/my_http_server

docker stats : pour avoir l'id du conteneur

docker exec 91d7c473c703 cat /usr/local/apache2/conf/httpd.conf > httpd.conf

http://localhost:8080/

docker run -d -p 8080:8080 --net=app-network --name mynewappli tatsuwo/mynewappli
-----------------------------
Reverse proxy

mettre dans un dossier public-html tous les fichiers voulus
http://localhost:8080/students/


pour lancer le docker compose :
regarder le docker-compose makefile et changer les noms des containers en consequence avec les fichiers impactés -> application.yml dans java et http_server -> httpd.conf
docker-compose up --build
docker-compose down
--------------------------------
docker login

mettre une image sur docker hub (image nom_image_sur_dockerhub) (tatsuwo car cest mon pseudo sur dockerhub) :
docker tag tatsuwo/my_postgres tatsuwo/my-database:1.0

docker push tatsuwo/my-database:1.0
