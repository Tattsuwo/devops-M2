mettre tous les workflows pour les pipelines dans le dossier .github/workflows

dans le main.yaml, il faut bien définir tous les uses et mettre la commande avec le chemin 
-> run: mvn clean verify --file TP1/java/simple-api-student-main/pom.xml


Ajouter les secrets du compte dockerhub dans actions/secrets de github

mettre la connection dockerhub avant le build :

- name: Login to DockerHub
  run: docker login -u ${{ secrets.DOCKERHUB_LOGIN }} -p ${{ secrets.DOCKERHUB_PASSWORD }}
  

mettre le push pour l'envoyer sur dockerhub :
# build on feature branches, push only on main branch
push: ${{ github.ref == 'refs/heads/main' }}


bien mettre les bonnes clés dans le main.yaml

NE PAS OUBLIER DE CREER LE BON REPERTOIRE AVEC LE BON TAG ET AJOUTER ":" A LA FIN POUR FAIRE COMPRENDRE A CE CON QUE CEST LE FICHIER ATTENDU
exemple : tags: ${{secrets.DOCKERHUB_LOGIN}}/tp-devops:simple-bdd


ATTENTION, quand on push, il faut prendre la bonne branche : ici cest main, mais ca pourrait être master.
 push: ${{ github.ref == 'refs/heads/main' }}
----------------------------------------

SONARCLOUD, ajouter avec maven pour executer le test de la qualité du code, il faut récupérer son nom d'organisation et le token sonnarcloud, ne pas oublier de bien mettre le chemin du dockerfile voulus


# finally build your app with the latest command
      - name: Build and test with Maven
        run: mvn -B verify sonar:sonar -Dsonar.projectKey=Tattsuwo_devops-M2 -Dsonar.organization=tattsuwo -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${{ secrets.SONAR_TOKEN }}  --file ./TP1/java/simple-api-student-main/pom.xml
