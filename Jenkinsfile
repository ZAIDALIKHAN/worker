pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        AWS_ACCOUNT_ID  = "218085830935"
        AWS_REGION      = "us-east-1"
        ECR_REGISTRY    = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/worker"
        IMAGE           = "${ECR_REGISTRY}:latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE} .
                """
            }
        }

        stage('Login to ECR') {
            steps {
                sh """
                    aws ecr get-login-password --region ${AWS_REGION} \
                    | docker login --username AWS --password-stdin ${ECR_REGISTRY}
                """
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh "docker push ${IMAGE}"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
    }
}
