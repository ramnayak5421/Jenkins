pipeline {
    agent any

    environment {
        DOCKER_PATH = "/usr/bin/docker" // Adjust if needed
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/VINAY-KUMAR-VANKAYALAPATI/jenikins-pipeline', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building application..."'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh 'sonar-scanner -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh '''
                $DOCKER_PATH build -t 2022bcd0023/myapp:latest .
                $DOCKER_PATH login -u 2022bcd0023 -p $DOCKER_PASS
                $DOCKER_PATH push 2022bcd0023/myapp:latest
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Cleaning up dangling images..."
            sh "$DOCKER_PATH system prune -f"
        }
        failure {
            echo "Pipeline failed. Please check logs."
        }
    }
}
