pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
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
        
        //stage('Publish to nexus') {
        //      steps {
        //        nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/com.vinaysdevopslab-0.0.8.war', type: 'war']], credentialsId: 'Nexus', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.0.232:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'AngelGoezLab-SNAPSHOT', version: '0.0.8'
        //      }
        //}

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
