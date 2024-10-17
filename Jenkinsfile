pipeline {
    agent any
    environment {
        REPO = '[KakaoTech-team20]/[Cloud_practice]/[springboot_alert]'
        ECR_REPO = '[211125697339].dkr.ecr.ap-northeast-2.amazonaws.com/[springboot_alert]'
        ECR_CREDENTIALS_ID = 'ecr:ap-northeast-2:moreburgerIAM'
        SSH_CREDENTIALS_ID = 'EC2_ssh_key'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "https://github.com/${REPO}.git", credentialsId: 'github_for_jenkins'
            }
        }

        stage('Build Jar') {
            steps {
                script {
                    sh './gradlew clean build'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${ECR_REPO}:latest", ".")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry("https://${ECR_REPO}", "$ECR_CREDENTIALS_ID") {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@${EC2_INSTANCE_IP} '
                    aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin ${ECR_REPO}
                    docker pull ${ECR_REPO}:latest
                    docker stop springboot_alert || true
                    docker rm springboot_alert || true
                    docker run -d --name springboot_alert -p 8080:8080 ${ECR_REPO}:latest
                    docker system prune -f
                    '
                    """
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
    }
}
