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
                script {
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "AngelGoezLab-SNAPSHOT" : "AngelGoezLab-RELEASE"
                    nexusArtifactUploader artifacts: [
                        [ 
                            artifactId: "${ArtifactId}", 
                            classifier: '', 
                            file: "target/${ArtifactId}-${Version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'Nexus', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.0.232:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"
                  }      
             }
        }
        
        stage('Show Environment variables') {
            steps {
                echo "'$ArtifactId' $Version $Name $GroupId"
            }
        }
        
        stage('Deploy') {
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Ansible_Controller', 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    execCommand: 'ansible-playbook /opt/playbooks/download-and-deploy.yaml -i /opt/playbooks/hosts', 
                                    execTimeout: 120000
                                )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false
                        )
                    ]
                )
            }
        }
        
         stage('Deploy Docker') {
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Ansible_Controller', 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    execCommand: 'ansible-playbook /opt/playbooks/download-and-deploy-docker.yaml -i /opt/playbooks/hosts', 
                                    execTimeout: 120000
                                )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false
                        )
                    ]
                )
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
