pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'shivammishra828'
        BACKEND_IMAGE = "${DOCKERHUB_USER}/dd-backend"
        FRONTEND_IMAGE = "${DOCKERHUB_USER}/dd-frontend"
    }

    stages {

        stage("Workspace Cleanup") {
            steps {
                script {
                    cleanWs()
                }
            }
        }

        stage("Pulling the Code") {
            steps {
                script {
                    git url: "https://github.com/ShivamMishra828/dd-assignment.git", branch: "main"
                }
            }
        }

        stage("Building the Code") {
            steps {
                script {
                        dir('backend') {
                            sh "docker build -t ${BACKEND_IMAGE}:latest ."
                        }
                    
                        dir('frontend') {
                            sh "docker build -t ${FRONTEND_IMAGE}:latest ."
                        }
                }
            }
        }

        stage("Pushing the image to Docker Hub") {
            steps {
                script {
                    withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh '''
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin

                        docker push ${BACKEND_IMAGE}:latest
                        docker push ${FRONTEND_IMAGE}:latest

                        docker logout
                    '''
                }
                }
            }
        }

        stage("Deploying the code to EC2") {
            steps {
                echo "Deploying the code with the help of docker..."
                sh "docker compose down && docker compose up --build -d"
            }
        }
    }
}
