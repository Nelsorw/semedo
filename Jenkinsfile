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
                    if (isUnix()) {
                        sh "docker build -t ${IMAGE_NAME}:latest ."
                    } else {
                        bat "docker build -t %IMAGE_NAME% ."
                    }

                    echo "Logging in to Docker Hub..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_creds', 
                                                     usernameVariable: 'DOCKERHUB_USER', 
                                                     passwordVariable: 'DOCKERHUB_PASS')]) {
                        if (isUnix()) {
                            sh 'echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin'
                        } else {
                            bat 'echo %DOCKERHUB_PASS% | docker login -u %DOCKERHUB_USER% --password-stdin'
                        }
                    }

                    echo "Pushing Docker image..."
                    if (isUnix()) {
                        sh "docker push ${IMAGE_NAME}:latest"
                    } else {
                        bat "docker push %IMAGE_NAME%"
                    }
                }
            }
        }
    }
}
