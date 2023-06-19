# TP2 Github Actions

## 2-1 What are testcontainers?

Testcontainers est une bibliothèque Java qui facilite l'écriture de tests d'intégration en fournissant des conteneurs temporaires pour les services externes tels que les bases de données, les serveurs web, etc. Ces conteneurs sont gérés automatiquement pendant l'exécution des tests cela nous permet de tester notre application dans un environnement isolé, reproductible et contrôlé, en garantissant que nos tests fonctionnent correctement avec les services externes.

## 2-2 Document your Github Actions configurations.

On ajoute tous les workflows pour les pipelines dans le dossier .github/workflows.
voir le fichier main.yml pour la documentation de chaque ligne.

Dans le main.yaml, il faut bien définir tous les uses et mettre le bon chemin dans les contextes.
-> run: mvn clean verify --file TP1/java/simple-api-student-main/pom.xml

J'ai ajouté les secrets du compte dockerhub dans actions/secrets de github

J'ai mis la connection dockerhub avant le build :

- name: Login to DockerHub
  run: docker login -u ${{ secrets.DOCKERHUB_LOGIN }} -p ${{ secrets.DOCKERHUB_PASSWORD }}
  

Il faut mettre le push pour l'envoyer sur dockerhub :
push: ${{ github.ref == 'refs/heads/main' }}

bien mettre les bonnes clés dans le main.yaml

NE PAS OUBLIER DE CREER LE BON REPERTOIRE AVEC LE BON TAG SUR DOCKERHUB ET AJOUTER ":" POUR DIRE QUE CEST LE FICHIER ATTENDU !!!
exemple : tags: ${{secrets.DOCKERHUB_LOGIN}}/tp-devops:simple-bdd

ATTENTION, quand on push, il faut prendre la bonne branche : ici cest main, mais ca pourrait être master.
 push: ${{ github.ref == 'refs/heads/main' }}


# TP2 SONARCLOUD

## Document your quality gate configuration.

Ajouter sonarcloud dans le run de maven pour executer le test de la qualité du code.
Il faut récupérer son nom d'organisation et le token sur sonarcloud ici (nom d'organisation tattsuwo et secrets.SONAR_TOKEN que j'ai mis sur Github)
ne pas oublier de bien mettre le chemin du dockerfile voulus,

run: mvn -B verify sonar:sonar -Dsonar.projectKey=Tattsuwo_devops-M2 -Dsonar.organization=tattsuwo -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${{ secrets.SONAR_TOKEN }}  --file ./TP1/java/simple-api-student-main/pom.xml

