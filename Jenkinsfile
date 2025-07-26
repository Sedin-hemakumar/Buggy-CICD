pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = 'ASIASJCHX6HERDGXFPSE'
    AWS_SECRET_ACCESS_KEY = 'FhVZ5Aoys/Vt7hO8Lm1gOafQsezjuW1F+0rpcGRU'
    AWS_SESSION_TOKEN = 'IQoJb3JpZ2luX2VjEDkaCmFwLXNvdXRoLTEiSDBGAiEA3ONC+sszQ50pnLuyAE+8O/afWdm8i/dI6uXL4Ud0gLcCIQDMf3QHARJIK1NW89hxT+AvOPPdudV6/BusA4wXqXJpRCqcAwhjEAAaDDE1NjkxNjc3MzMyMSIM1FWEXaQx3ZlDX2ckKvkCjEj3KQgzV3HMzoFwebnSh4pp87fXB8unhwhbdw5QS3S1JGzqMJj1eXM1+TlHt+Tv3wPUvjtGL21KaI1DT1lsEEAghod4zKl6pMgXe8xZEwrY3EnjF3ZhbyRPUc+bOdrGchVZXmCyBoXZJ3jr2Zo4A9KtEzXfeWBcjjotvY1kRyjpBKvoL/clUuOZqzdlfFh7X19wza3bXzz60s3qhLwkaVl3vD8aZTkufrA4vr/qZ8G6h96s9mzdauJu3BTDTsq6q4gkM+nokVhzsei6xcT2IL1zHXHizVHU/Q8yPcHssL9f+M905xg+6hHSR8gT/cigFMEiOTg9ciq5qxOulzu7wTEa/RJPklijIzMGj0y3W8pwRXNR3KNze63e/dTChgrOc9cSvXCoUO8W1swGvgprHhggxummuKVdMYbDWC4v5hBMam5+miIVItvaeDAZ+sjE6PkLYRVHFYStoHd3xonGTL4Hdp8lZqkEQauYTl2vfjFwedCNlTK6h4EwyZ+UxAY6pQGX4LcafvegQMdmT9lbxmF7SM1S3RUWSyPIhAwDa6qPW0qLJJJrMbvYmjaUn74eMhdtx1N0/0D626dSoKKtK+b00bx1AushVZwc1zMwwk2YnKkG+xBcKrIW+Ar1LrtE2g4+ywDytbWgUlJfMpNc6oDQAWljizNR1gG+yr9eWw8cppRWSo+B6VtFtgja2fWh/Vi53jv7kGRL65Tr7nCFNkIxJeNF+pM='
    AWS_REGION = 'ap-south-1'
    IMAGE_TAG = 'latest'
    ECR_REGISTRY = '156916773321.dkr.ecr.ap-south-1.amazonaws.com'
    IMAGE_NAME = 'hemath-rails'
  }

  stages {
    stage('Checkout Codes') {
      steps {
        git branch: 'Hemath', url: 'https://github.com/Sedin-hemakumar/Buggy-CICD.git'
      }
    }

    stage('Docker Login to ECR') {
      steps {
        sh '''
          export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
          export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
          export AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN}

          aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}
        '''
      }
    }

    stage('Build Docker Image') {
  steps {
    sh '''
      docker build --no-cache -f Dockerfile.app -t ${IMAGE_NAME}:${IMAGE_TAG} .
      docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
    '''
  }
}


    stage('Push Image to ECR') {
      steps {
        sh '''
          docker push ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
        '''
      }
    }
    stage('Run with Docker Compose') {
  steps {
    sh '''
      docker rm -f Buggy_rails_app || true
      docker-compose down || true
      docker-compose up -d
    '''
  }
}

  }
}
