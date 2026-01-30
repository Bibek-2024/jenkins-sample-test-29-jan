pipeline {
    agent any

    environment {
        APP_SERVER_IP = "REPLACE_APP_SERVER_IP"
        IMAGE_NAME = "sample-java-app"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Bibek-2024/jenkins-sample-test-29-jan.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Send Files to App Server') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no target/*.jar ubuntu@${15.206.74.246}:/home/ubuntu/
                scp -o StrictHostKeyChecking=no Dockerfile ubuntu@${15.206.74.246}:/home/ubuntu/
                '''
            }
        }

        stage('Run Docker on App Server') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@${APP_SERVER_IP} << EOF
                  sudo docker rm -f app || true
                  sudo docker build -t app .
                  sudo docker run -d -p 80:8080 --name app app
                EOF
                '''
            }
        }
    }
}
