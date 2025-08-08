pipeline {
    agent any
    environment {
        REPO = 'https://github.com/Become-DevOps/proj.git'
        IMAGE = 'static-web'
        PORT = '8080:80'
    }
    stages {
        stage('Clone Project') {
            steps {
                git "${REPO}"
            }
        }
        stage('Install Docker on QA') {
            steps {
                sh 'ansible web -m apt -a "name=docker.io state=present" --become'
            }
        }
        stage('Build Docker Image on QA') {
            steps {
                sh 'ansible web -m copy -a "src=. dest=/home/sree/proj"'
                sh 'ansible web -m shell -a "docker build -t ${IMAGE} /home/sree/proj"'
            }
        }
        stage('Run Docker Container on QA') {
            steps {
                sh 'ansible web -m shell -a "docker rm -f ${IMAGE} || true"'
                sh 'ansible web -m shell -a "docker run -d -p ${PORT} ${IMAGE}"'
            }
        }
    }
}
