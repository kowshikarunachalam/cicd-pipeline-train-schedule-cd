pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeploytoStaging'){
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
            
                sshPublisher(
                   continueOnError: false, failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                        configName: 'staging',
                        sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                         ], 
                    transfers: [
                        sshTransfer(
                        sourceFiles: "dist/trainSchedule.zip",
                        removePrefix: 'dist/',
                        remoteDirectory: '/tmp',
                        execCommand: 'unzip /tmp/trainSchedule.zip'
                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        
    } }
