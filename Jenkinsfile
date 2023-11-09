pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    environment {
        AWS_ACCOUNT_ID="334210602318"
        AWS_DEFAULT_REGION="ap-southeast-1"
        IMAGE_REPO_NAME="javatechie"
        IMAGE_TAG="v1"
        REPOSITORY_URI = "334210602318.dkr.ecr.ap-southeast-1.amazonaws.com/devops_automation"
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/baotg/devops_automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t javatechie/devops-integration .'
                }
            }
        }
        stage('Logging into AWS ECR'){
            steps{
                script{
                    sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
            }
        }
        stage('Push image to ECR'){
            steps{
                script{
                   sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                   h """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
                }
            }
        }
    }
}