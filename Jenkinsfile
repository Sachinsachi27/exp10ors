
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "anil-demo-app"

        PYTHON = "C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python313\\python.exe"

        DOCKER = "C:\\Users\\STUDENT\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                echo "Checking Python installation..."

                bat '"%PYTHON%" --version'

                echo "Verifying Python syntax..."

                bat '"%PYTHON%" -m py_compile app.py'

                echo "Python syntax verification successful!"
            }
        }

        stage('3. Docker Verification') {
            steps {
                echo "Checking Docker installation..."

                bat '"%DOCKER%" --version'

                echo "Checking Docker Engine status..."

                bat '"%DOCKER%" info'
            }
        }

        stage('4. Build & Tag Docker Image') {
            steps {
                echo "Building Docker image..."

                bat '"%DOCKER%" build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .'
            }
        }

        stage('5. Push Image to Registry') {
            steps {
                echo "Pushing versioned image..."

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                echo "Pushing latest image..."

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
            echo "Latest: ${REGISTRY}/${IMAGE_NAME}:latest"
        }

        failure {
            echo "Pipeline failed. Please check Jenkins console output."
        }
    }
}
