# TP1 Database

## 1-1 Document your database container essentials: commands and Dockerfile.

j'ai créé 3 répertoires pour les différentes images qu'on va vouloir créer : http_server, java et bdd
en mettant le Dockerfile dans chacun des dossiers pour générer l'image.

La commande de build et run de l'image pour la bdd posgres et lancement de l'image :

docker build -t tatsuwo/my_postgres .
docker run -d -p 8080:5432 --net=app-network --name my_postgres tatsuwo/my_postgres
docker network create app-network

Dans la majorité des cas, on ne change pas le port à droite. On ne change que le port à gauche qui est le port local qu'on attribut.
le --net=app-network permet de creer une zone fermer avec tous les images contenant le network.

ajout des lignes pour copier et utiliser le script sur la sortie /docker-entrypoint-initdb.d : 
COPY CreateScheme.sql /docker-entrypoint-initdb.d
COPY InsertData.sql /docker-entrypoint-initdb.d

lancement de la BDD : 
docker run -p 8090:8080 --net=app-network --name=adminer -d adminer

retirer une image : 
docker rm -f my_postgres

Enlever la persistance des données :
docker run -d -p 8080:5432 --net=app-network -v /my/own/datadir:/var/lib/postgresql/data --name my_postgres tatsuwo/my_postgres

la config du serveur : 
serveur : my_postgres
utilisateur : usr
mdp : pwd
bdd : db

# TP1 Backend API

Dockerfile : 

FROM openjdk
COPY Main.class /
ENTRYPOINT ["java"] 
CMD ["Main"]

Pour build l'image du backend java
docker build -t tatsuwo/myjava .

Create your Springboot application :
https://start.spring.io/

Use the following config:
    Project: Maven
    Language: Java 17
    Spring Boot: 2.7.5
    Packaging: Jar
    Dependencies: Spring Web


Ajout du code dans GreetingController.java à côté du fichier SimpleapiApplication.java
docker build -t tatsuwo/myappli .
docker run -d -p 8080:8080 tatsuwo/myappli --name myappli tatsuwo/myappli


## 1-2 Why do we need a multistage build? And explain each step of this dockerfile

Le multistage build permet de séparer les environnements de construction et d'exécution, améliore la sécurité et l'efficacité, réduit la taille de l'image et simplifie le processus de construction. Il est très utile lors de la construction d'applications complexes ou lorsqu'il y a des exigences différentes entre les étapes de construction et d'exécution.

Explication de chaque étape du dockerfile : 
# Build
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build
Cette ligne spécifie l'image de base à utiliser pourl'étape de construction. Ici l'image de base est maven:3.8.6-amazoncorretto-17 qui contient Maven et l'environnement d'exécution Amazon Corretto JDK 17.

ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
Ces deux instructions définissent une variable d'environnement MYAPP_HOME avec la valeur /opt/myapp et définissent ensuite le répertoire de travail dans le conteneur sur /opt/myap


COPY pom.xml .
COPY src ./src
Ces deux instructions copient le fichier pom.xml et le répertoire src local vers le répertoire du conteneur "." et "./src"


RUN mvn package -DskipTests
Cette instruction exécute la commande Maven mvn package dans le répertoire du conteneur.

# Run
FROM amazoncorretto:17
Cette ligne spécifie l'image de base à utiliser pour cette étape d'exécution. Ici, l'image de base est amazoncorretto:17, qui contient l'environnement d'exécution Amazon Corretto JDK 17.

ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
Même chose que précédemment


COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar
Cette instruction copie le fichier JAR de l'application généré dans l'étape précédente ($MYAPP_HOME/target/*.jar) depuis l'étape de construction vers le répertoire  du conteneur ($MYAPP_HOME) et le renomme en myapp.jar.

ENTRYPOINT java -jar myapp.jar
Cette instruction définit la commande d'entrée du conteneur. il exécute la commande java -jar myapp.jar






Je met les config de la BDD dans le fichier yml

docker build -t tatsuwo/mynewappli .

Je veille à avoir un port de sortie différents pour éviter les conflits : 
docker run -d -p 8070:5432 --net=app-network --name my_postgres tatsuwo/my_postgres
docker run -p 8090:8080 --net=app-network --name=adminer -d adminer
docker run -d -p 8060:8080 --net=app-network --name mynewappli tatsuwo/mynewappli

http://localhost:8060/


# TP1 Http server

docker build -t tatsuwo/my_http_server .
docker run -d -p 8080:80 --net=app-network --name my_http_server tatsuwo/my_http_server

docker stats : pour avoir l'id du conteneur

je récupére la config du conteneur :
docker exec 91d7c473c703 cat /usr/local/apache2/conf/httpd.conf > httpd.conf

et j'ajoute la modification dans le dockerfile pour prendre en compte le changement.


http://localhost:8080/

docker run -d -p 8080:8080 --net=app-network --name mynewappli tatsuwo/mynewappli


# TP1 Reverse proxy

mettre dans un dossier public-html tous les fichiers voulus

http://localhost:8080/students/

# TP1 Link application

## 1-3 Document docker-compose most important commands. 1-4 Document your docker-compose file.

pour lancer le docker compose :
regarder le docker-compose makefile et changer les noms des containers en consequence avec les fichiers impactés -> application.yml dans java et http_server -> httpd.conf

pour démarrer le build de tous les dockers :
docker-compose up --build

pour arrêter l'exécution de tous les dockers :
docker-compose down

pour se connecter au dockerhub :
docker login

mettre une image sur docker hub (image nom_image_sur_dockerhub) (tatsuwo car cest mon pseudo sur dockerhub) :
docker tag tatsuwo/my_postgres tatsuwo/my-database:1.0

docker push tatsuwo/my-database:1.0


La documentation du docker-compose a été faite directement sur le fichier dans le dossier TP1
