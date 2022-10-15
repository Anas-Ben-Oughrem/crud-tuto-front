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
                nodejs(nodeJSInstallationName: 'nodejs'){
                    withSonarQubeEnv('sonar'){
                        sh 'npm install sonar-scanner'
                        sh 'npm run sonar'
                        }
                    }
                }
            }
        
        /*stage("Quality gate status check") {
            steps {
                script{
              timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if(qg.status != 'OK') {
                    slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#jenkins-notifications', color: 'danger', message: 'Pipeline aborted: Sonarqube Analysis marked as failed', teamDomain: 'Legion14', tokenCredentialId: 'slack-channel'
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
                }
              }
            }
          }*/
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
        stage('Build And Deploy Docker Image'){
            steps{
                script{
                    echo "deploying the application"
                    withCredentials([usernamePassword(credentialsId:'nexus-docker-repo',usernameVariable:'USER',passwordVariable:'PWD')]) {
                        sh "echo $PWD | docker login -u $USER --password-stdin localhost:8083"
                        sh "docker build -t localhost:8083/angular-app:1.0 ."
                        sh "docker push localhost:8083/angular-app:1.0"

                }
            }
        }
    }
}
}
