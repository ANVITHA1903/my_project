pipeline {
    agent any

    environment {
        QA_USER = "anvitha"
        QA_HOST = "13.201.93.30"
        QA_PORT = "22"
        IMAGE_NAME = "static-web"
        CONTAINER_PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Become-DevOps/proj.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Push to QA Server & Deploy') {
            steps {
                sshagent(['qa-server-ssh-key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${QA_USER}@${QA_HOST} '
                        cd proj || git clone https://github.com/Become-DevOps/proj.git && cd proj &&
                        git pull &&
                        docker build -t ${IMAGE_NAME} . &&
                        docker rm -f ${IMAGE_NAME} || true &&
                        docker run -d -p ${CONTAINER_PORT}:80 --name ${IMAGE_NAME} ${IMAGE_NAME}
                    '
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
