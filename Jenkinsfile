pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="517056076755"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="jenkinsdf"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
                 
            }
        }
        
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '569b9009-82cc-42c6-8aa4-142148941f7f', url: 'https://github.com/ofekbs777/Bootcamp.git']]])   
            }
        }

  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
            
        sh "cd HELM_PY_proj/"
          
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}" 
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
         }
        }
      }
    }
}
