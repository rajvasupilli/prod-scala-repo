pipeline {
    agent any 
    stages {
        stage('Install Pre-requisites') {
            steps {
                echo 'Installling the prerequisites!!!'
                sh '''
                      #bash prereq.sh
                   '''
            }
        }

       stage('Push image from Staging to Production ECR') {
            steps {
                echo 'Build,Tag and Push the Docker Image into the ECR'
                sh ''' aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 249147895833.ecr.us-east-1.amazonaws.com
                       sudo docker pull 249147895833.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest
                       export AWS_PROFILE=secondary
                       aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 667333752349.dkr.ecr.us-east-1.amazonaws.com
                       sudo docker tag 249147895833.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest 667333752349.dkr.ecr.us-east-1.amazonaws.com/prod-scala-image-repo:latest
                       sudo docker push 667333752349.dkr.ecr.us-east-1.amazonaws.com/staging-scala-image-repo:latest    
                   '''
                }    
            }
        }
    }
