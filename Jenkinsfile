pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1' // Change to your desired region
        ECR_REPOSITORY = 'hello-world-app' // Your ECR repository name
        IMAGE_TAG = "${env.BUILD_ID}" // Using Jenkins build ID as the image tag
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/SuryaGopilli/Hello-world-app' // Replace with your GitHub repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${ECR_REPOSITORY}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    // Login to ECR
                    def loginCommand = "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin 8517252526025.dkr.ecr.${AWS_REGION}.amazonaws.com"
                    sh loginCommand
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag the Docker image
                    sh "docker tag ${ECR_REPOSITORY}:${IMAGE_TAG} 8517252526025.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push the Docker image to ECR
                    sh "docker push 8517252526025.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed to ECR successfully!'
        }
        failure {
            echo 'There was an error during the pipeline execution.'
        }
    }
}
