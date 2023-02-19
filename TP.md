# GoSecuri

Calvin Djafari

# Remarques 

- build du code -> OK
- exécution de tests unitaires (si le projet en fourni), et de tests statiques -> OK
- génération d'un package -> OK
- publication du package sur Nexus -> OK (utilisation de plugin tels que : Nexus Artifact Uploader et Pipeline Utility Steps)  
Variables importantes dans le JenkinsFile :  
NEXUS_URL = "0.0.0.0:8081" // adresse du serveur  
NEXUS_REPOSITORY = "gosecuri2" // nom du repo  
NEXUS_CREDENTIAL_ID = "NEXUS_CRED" // login (définie dans les utilisateurs Jenkins pour se connecter à Nexus)  
- SonarQube mis en place -> OK  
Variables importantes dans le script :   
sonar.login=admin // login   
sonar.password=adminadmin // login   
sonar.organization=sonarqube // id d'autentification pour enregistrer dans sonarqube  


preuves disponibles dans le dossier : preuves (trace d'execution et de config) 

# Run 

Ligne de commande pour lancer la génération des pages htmls : java -jar target/gosecuri-0.0.1.jar depot/ depot/idcards/ depot/staffs/
