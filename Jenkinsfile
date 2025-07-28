pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = 'ASIASJCHX6HEVPY2FOJ6'
    AWS_SECRET_ACCESS_KEY = 'C3bnYL+4s5FL244C3NtoZYOqPFKeoEtakD+RY4Fd'
    AWS_SESSION_TOKEN = 'IQoJb3JpZ2luX2VjEF0aCmFwLXNvdXRoLTEiRzBFAiEAyr1jixNoM98qZK3+BkpRC2dRtUE9it898XU3WRYdTAECIBhDArBNEydAHS42or8pI9iO4Bw7ym13if644LKGq/cGKqUDCIb//////////wEQABoMMTU2OTE2NzczMzIxIgzcRJtKxcPHp2XJswoq+QKNmg5URZ0lyCczalGxS7YTdl24R6SpIKSNFHbiQoP/nOijPTyKNVkrriVT3CTBRjKhzjWA3BxP0D4DX9rrNgthmG1hlmnurYfblRiOh/hsyUil457v0tK3qEf6OmWhPl+SgpN+4abBeO6kpJPZQyFWBON9qUUOreZk+zj0o8U6JX90DUS2rC8h5tqtgl+tUP4jWIMi1Hpx3WlMTVj7vutw+fRmdWoMRUrSH9FveFvgtVBNvaH8Wo014/ndTT/uSP8DnQSTxpWE+SQszU/5ZsXBLSO4QOHG79qDFPtfVrbkBIsJCp7fZPs8Ke6kPkE8jNbPUZzIoBHb9Vkj6pQSYZT/Fr38P2Yyf4N0YtZqXDHx48XpiLGS1evMM4V/CUi1ON9dlsve4EZWspWYO1pIaecol6xmDYAf/2rXD0rXZF9E+NPYpZpcRaPEwvueN4PLdtviGjlgkoL4tadZ58j4f+DgmcyO7amcHGj44ubvSv3uDeoLZ20TWs+eWDCMkZzEBjqmAaIrvoyw2s/OeEVzATC9m8GTJ91O6yhIliOnMfX3ExytvMxjpy6YW+SsZDm0cChHTjNuTvRa2JU/VbCWvVHf5KATyHzddiQjY3wmdSgQL3gU1WtPdLaj/bAybZMdipj4un8XvGlgZwPnXZy4l/jD9gNqU/x/INH7fxcZ06ov7IfGdNLAnk7SUVH5k/kDQkGaWhDlsJpVYDZpwEOSNmJeLhvv8loqpX8='
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
