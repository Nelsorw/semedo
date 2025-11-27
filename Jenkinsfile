pipeline {
    agent any

    environment {
        IMAGE_NAME = 'semed01/node-web-app'
    }

    stages {

        stage('Build') {
            steps {
                echo "Building the project..."
                script {
                    if (isUnix()) {
                        sh 'ls -la'
                    } else {
                        bat 'dir'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:latest ."

                    echo "Logging in to Docker Hub..."
                    sh """
                    echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    """

                    echo "Pushing Docker image..."
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
