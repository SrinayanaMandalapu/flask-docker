pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')   // Add Docker Hub credentials in Jenkins
        IMAGE_NAME = "srinayana20/flask-docker"         // Replace with your Docker Hub repo name
        IMAGE_TAG = "latest"
        PYTHON = 'C:\\Users\\manda\\AppData\\Local\\Programs\\Python\\Python312'
        PATH = "${PYTHON};${PYTHON}\\Scripts;${env.PATH}"
    }


    stages {

        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SrinayanaMandalapu/flask-docker.git'
            }
        }

        stage('Install Dependencies') {
          steps {
            bat """
              "%PYTHON%\\python.exe" -m pip install --upgrade pip
              "%PYTHON%\\python.exe" -m pip install -r requirements.txt
            """
          }
        }
        
        stage('Run Tests') {
          steps {
            bat """
              "%PYTHON%\\python.exe" -m py_compile app.py
            """
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
            withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                      usernameVariable: 'DOCKER_USER',
                                      passwordVariable: 'DOCKER_PSW')]) {
                  bat """
                    echo Logging in to Docker Hub...
                    echo %DOCKER_PSW% | docker login --username %DOCKER_USER% --password-stdin
            
                    echo Pushing image ${IMAGE_NAME}:${IMAGE_TAG} ...
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
            
                    echo Logging out...
                    docker logout
                  """
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
