pipeline {
    agent any
    tools{
        //jdk "Java8"
        maven "MAVEN"
    }
    
    
    environment {
            NEXUS_VERSION = "nexus3"
            NEXUS_PROTOCOL = "http"
            NEXUS_URL = "0.0.0.0:8081"
            NEXUS_REPOSITORY = "gosecuri2"
            NEXUS_CREDENTIAL_ID = "NEXUS_CRED"
            
            SCANNER_HOME = tool 'sonarqube'
        }
    stages {
        stage("Récuperation des sources"){
            steps{
                git url: 'https://github.com/CalD2/gosecuri2.git',
                branch: 'main'
            }
        }

        stage("Build"){
            steps{
                bat """
                java -version

                mvn clean install -DskipTests 
                """
            }
        }

        stage("Test"){
            steps{
                bat """
                java -version

                mvn test
                """
            }
            post{
                success{
                    archiveArtifacts 'target/*.jar'
                }
            }

        }
        
        
        
        
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    a = 1;
                    echo a.toString();
                    pom = readMavenPom file: "pom.xml";
                    echo pom.toString();
                    
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                    
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps 
            {
                withSonarQubeEnv('SonarQubeServ')
                {
                     bat """ mvn verify sonar:sonar -D sonar.login=admin -D sonar.password=adminadmin -D sonar.organization=sonarqube -D sonar.projectKey=project -D sonar.binaries=target/classes
                """
                }
            }
        }
        
    }


}