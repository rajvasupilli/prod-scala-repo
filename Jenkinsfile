pipeline {
    agent any
    
    parameters {
          text(name: 'STAGING_ACCOUNT_ID', defaultValue: '406370147020', description: 'Enter the AWS Account ID of Staging Environment')
          text(name: 'PROD_ACCOUNT_ID', defaultValue: '667333752349', description: 'Enter the AWS Account ID of Production Environment')
        string(name: 'REGION', defaultValue: 'us-east-1', description: 'Enter the Region')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Enter the tag pertaining to the ECR Image')
        string(name: 'STAGING_REPO_NAME', defaultValue: 'staging-scala-image-repo', description: 'Enter the AWS ECR Repo name pertaining to Staging Environment')
        string(name: 'PROD_REPO_NAME', defaultValue: 'prod-scala-image-repo', description: 'Enter the AWS ECR Repo name pertaining to Production Environment')        
    }
    
    stages {
        stage('Install Pre-requisites') {
            steps {
                echo 'Installing the prerequisites!!!'
                sh '''
                      #bash prereq.sh
                   '''
            }
        }

       stage('Push image from Staging to Production ECR') {
            steps {
                echo 'Build,Tag and Push the Docker Image into the ECR'
                sh """ aws ecr get-login-password --region ${params.REGION} | sudo docker login --username AWS --password-stdin ${params.DEV_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com
                       sudo docker pull ${params.DEV_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com/${params.STAGING_REPO_NAME}:${params.IMAGE_TAG}
                       export AWS_PROFILE=secondary
                       aws ecr get-login-password --region ${params.REGION} | sudo docker login --username AWS --password-stdin ${params.STAGING_REPO_NAME}.dkr.ecr.${params.REGION}.amazonaws.com
                       sudo docker tag ${params.STAGING_REPO_NAME}.dkr.ecr.${params.REGION}.amazonaws.com/${params.STAGING_REPO_NAME}:${params.IMAGE_TAG} ${params.PROD_ACCOUNT_ID}.dkr.ecr.${params.REGION}.amazonaws.com/${params.PROD_REPO_NAME}:${params.IMAGE_TAG}
                       sudo docker push ${params.PROD_REPO_NAME}.dkr.ecr.${params.REGION}.amazonaws.com/${params.PROD_ACCOUNT_ID}:${params.IMAGE_TAG}    
                   """
                }    
            }
        }
    }
