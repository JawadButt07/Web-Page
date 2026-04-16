pipeline {
    agent any

    environment {
        IMAGE_NAME = "mini-web:1.0"
        CONTAINER_NAME = "mini-web"
        PORT = "8082"
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/JawadButt07/Web-Page.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh '''
                        docker run --rm \
                        -v $WORKSPACE:/usr/src \
                        sonarsource/sonar-scanner-cli \
                        -Dsonar.projectKey=mini-web \
                        -Dsonar.sources=/usr/src \
                        -Dsonar.host.url=http://sonarqube:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
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
            echo "✅ CI/CD SUCCESS: Build + Sonar + Deploy Done"
        }
        failure {
            echo "❌ Pipeline Failed - Check Logs"
        }
    }
}
