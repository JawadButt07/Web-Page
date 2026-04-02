pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'YOUR_REPO_URL'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('mini-web:1.0')
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo "No real tests for static page, but SonarQube scan can go here"
            }
        }

        stage('Push Image') {
            steps {
                echo "Push to DockerHub (optional)"
            }
        }
    }
}

stage('SonarQube Analysis') {
   step {
       withSoanrQubeEnv('SonarQubeServer') {
            sh  'sonar-scanner -Dsonar.projectKey=mini-web -Dsonar.sources=.'
          }

      }

   }
                      

