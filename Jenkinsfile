pipeline {
    agent any

    environment {
        IMAGE_NAME = "mini-web:1.0"
        CONTAINER_NAME = "mini-web"
        PORT = "8082"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/JawadButt07/Web-Page.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d -p $PORT:80 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful"
        }
        failure {
            echo "❌ Build Failed - Check Logs"
        }
    }
}
