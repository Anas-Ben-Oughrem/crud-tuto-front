pipeline {
    agent any
    stages{
        stage('Init'){
            steps{
                script{
                    sh 'npm install --force'
                }
            }
        }
        stage('Compile'){
            steps{
                script{
                    sh 'ng build'
                }
            }
        }
        stage('Sonarqube Analysis'){
            steps{
                script{
                    sh 'npm run sonar'
                    }
                }
            }
        
        stage('Email Notification'){
            steps{
                script{
                    mail bcc: '', body: '''Hi,
Welcome to jenkins email alerts.
Thanks,
Anas''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'anasbo7@hotmail.com'
                }
            }
        }
        stage('Slack Notification'){
            steps{
                script{
                    slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#jenkins-notifications', color: 'good', message: 'Welcome to jenkins notifications channel, legionaries. Sent from Jenkins', teamDomain: 'Legion14', tokenCredentialId: 'slack-channel'
                }
            }
        }
    }
}
