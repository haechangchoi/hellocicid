pipeline {
    agent any

    tools {
        gradle 'gradle-latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/haechangchoi/hellocicid.git'
            }
        }
        stage('Build') {
            steps {
                bat './gradlew clean build'
            }
        }
        stage('Test') {
            steps {
                bat './gradlew test'
            }
        }
        stage('Docker Build and Run') {
            steps {
                script {
                    // Docker 명령 실행
                    bat '''
                    docker build -t hellocicid-app .
                    docker stop hellocicid-container || true
                    docker rm hellocicid-container || true
                    docker run -d -p 8080:8080 --name hellocicid-container hellocicid-app
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed."
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline execution failed. Please check the logs."
        }
    }
}
