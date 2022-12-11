pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    
    environment {
        artifactId = readMavenPom().getArtifactId()
        version    = readMavenPom().getVersion()
        name       = readMavenPom().getName()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
        
        stage('Publish to nexus') {
              steps {
                nexusArtifactUploader artifacts: [
                    [ 
                        artifactId: 'VinayDevOpsLab', 
                        classifier: '', 
                        file: 'target/VinayDevOpsLab-0.0.8-SNAPSHOT.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: 'Nexus', 
                groupId: 'com.vinaysdevopslab', 
                nexusUrl: '172.20.0.232:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'AngelGoezLab-SNAPSHOT', 
                version: '0.0.8-SNAPSHOT'
              }
        }
        
        stage('Show Environment variables') {
            steps {
                echo "'$artifactId' $version $name"
            }
        }

       // Stage3 : Publish the source code to Sonarqube
       // stage ('Sonarqube Analysis'){
       //     steps {
       //         echo ' Source code published to Sonarqube for SCA......'
       //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
       //              sh 'mvn sonar:sonar'
       //         }
       // 
       //     }
       // }

        
        
    }

}
