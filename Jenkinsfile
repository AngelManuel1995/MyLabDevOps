pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    
    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version    = readMavenPom().getVersion()
        Name       = readMavenPom().getName()
        GroupId    = readMavenPom().getGroupId()
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
                        artifactId: "${ArtifactId}", 
                        classifier: '', 
                        file: 'target/VinayDevOpsLab-0.0.8-SNAPSHOT.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: 'Nexus', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.0.232:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'AngelGoezLab-SNAPSHOT', 
                version: "${Version}"
              }
        }
        
        stage('Show Environment variables') {
            steps {
                echo "'$ArtifactId' $Version $Name $GroupId"
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
