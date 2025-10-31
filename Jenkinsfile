pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')   // Add Docker Hub credentials in Jenkins
        IMAGE_NAME = "srinayana20/flask-docker"         // Replace with your Docker Hub repo name
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SrinayanaMandalapu/flask-docker.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running basic test...'
                bat 'python -m py_compile app.py'  // Simple syntax check for demo
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    echo "Logging in and pushing Docker image..."
                    bat "docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}"
                    bat "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    bat "docker logout"
                }
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs!"
        }
        always {
            cleanWs()
        }
    }
}
