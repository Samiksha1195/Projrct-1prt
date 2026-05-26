pipeline {
    agent {
        label 'agent'
    }

    stages {

        stage('Clone GitHub Repo') {
            steps {
                git 'https://github.com/Samiksha1195/Projrct-1prt.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t prt-cicd-app .'
            }
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
