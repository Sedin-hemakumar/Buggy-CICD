pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = 'ASIASJCHX6HE5JTFWDHZ'
    AWS_SECRET_ACCESS_KEY = 'EXRTHfbxz2LqvE4EAi7Fwnq1Ln4VyiSmWbso9ZK5'
    AWS_SESSION_TOKEN = 'IQoJb3JpZ2luX2VjEEgaCmFwLXNvdXRoLTEiSDBGAiEAzY2QZgB/x+njwIc4paM9rjlpgJstqCVWubm0T8Xjc+YCIQD8pBx0nKLzXC82d4O6vHW0ysEJFKwAK6XlV72YvM0bKyqcAwhxEAAaDDE1NjkxNjc3MzMyMSIMBnyADSmEpAJhUjxvKvkCDTHNhwd1ZRNWVlSbm4eIVyab8VuL8uNVDCX0eSxMHh5RMY1TDdTLvv6Vpj6I38pL8lEBfFw8hs21Fg/mmCWeA/IaGx8K9u5HxJX3QnspvpGG+8njkx5bLJppww74cmXw3Ze2LU6Al7KO4y5zdBDfwVIK33t+9BRMvGEnsRVOK6SKkkuyFb5nPgheLBDy71Poheqo5vGsk9KLvXj6dnw+EhGVw75/Z+RTu/jhhMa1kkEK3O5PtyxaEY3EmsKMPRGUGMzJJHlnqWB43MO+wZzN/f2ahtHosiSag1im+TwotxPIelOUXaZ/JFbcAtTfAE9Nfe41gi/kTdGhYnTjVy2w+1vhMNcCv3hIzkn6qDe2XL8X0nBasFlKohImVctYaxdjU8Lk9g+HBtl+E17slXClWYcb4i4XkJsQmf+4CDwOFtBZhXXJiKMZg94vbE5W1MMCu6CxagS91DOdDHXP2pIRDFpkmv1ao4Fip6zv0VEn31MWeXI4X+h0VYYwi7CXxAY6pQGMqjIG1jr2+d9JZMh8CfV8CzjQZvmzzdZqK0z3tCxzyJlGjF1oZzy/F1cAJSRI1wP3RLC8SJRunVizj0g/x6Svb2nbAdMoXn9VJrvYuvwQRGViifaDeVmC2a8IcNQjxWdSrkYohxXgAU/Dshe8qBJhb7EYe46OdsT4ZjhotZZQg0CdrMngLXMHITbB/EMpjKT5jClxq+7wVJQVWCfKak8f9u5cgiQ='
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
