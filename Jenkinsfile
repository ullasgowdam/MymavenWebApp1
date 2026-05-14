pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                url: 'https://github.com/ullasgowdam/MymavenWebApp1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war',
                fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp target/*.war /opt/tomcat/webapps/'
            }
        }

        stage('Restart Tomcat') {
            steps {
                sh '''
                /opt/tomcat/bin/shutdown.sh || true
                sleep 5
                /opt/tomcat/bin/startup.sh
                '''
            }
        }
    }

    post {

        success {
            echo 'Build and Deployment Successful!'
        }

        failure {
            echo 'Build Failed!'
        }
    }
}
