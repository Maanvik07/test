pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("your-image-name:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'docker-credentials-id') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(['ssh-credentials-id']) {
                    sh 'ssh user@server "docker pull your-image-name:${env.BUILD_ID} && docker stop old-container && docker rm old-container && docker run -d -p 80:80 --name new-container your-image-name:${env.BUILD_ID}"'
                }
            }
        }
    }
}
