pipeline {
    agent {
        label 'agent'
    }
    environment {
        IMAGE_NAME = "Samiksha1195/prt-app:v1"
    }

    stages {

        stage('Clone GitHub Repo') {
            steps {
                git 'https://github.com/Samiksha1195/Projrct-1prt.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

       stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }


        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f prt-container || true
                docker run -d --name prt-container -p 8085:80 prt-cicd-app
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                sleep 10
                curl http://localhost:8085
                '''
            }
        }
    }
}
