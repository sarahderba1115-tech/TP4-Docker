pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'sarah428'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build images') {
            steps {
                bat 'docker build -t %DOCKERHUB_USER%/mern-server:latest ./Server'
                bat 'docker build -t %DOCKERHUB_USER%/mern-client:latest ./Client'
            }
        }

        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'U', passwordVariable: 'P')]) {
                    bat 'echo %P%| docker login -u %U% --password-stdin'
                    bat 'docker push %DOCKERHUB_USER%/mern-server:latest'
                    bat 'docker push %DOCKERHUB_USER%/mern-client:latest'
                }
            }
        }

        stage('Run container') {
            steps {
                bat 'docker rm -f mern-test || exit 0'
                bat 'docker run -d --name mern-test -p 9001:9000 %DOCKERHUB_USER%/mern-server:latest'
                bat 'docker ps'
            }
        }
    }
}
