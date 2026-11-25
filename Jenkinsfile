
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "student-demo-app"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                echo "Verifying Python syntax..."
                bat 'py -3 -m py_compile app.py'
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                echo "Building Docker image..."

                bat """
                    docker build -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} -t ${REGISTRY}/${IMAGE_NAME}:latest .
                """
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                echo "Pushing Docker images to registry..."

                bat "docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                bat "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }

        failure {
            echo "Pipeline failed! Check the Jenkins console output."
        }

        always {
            echo "Jenkins pipeline execution completed."
        }
    }
}
