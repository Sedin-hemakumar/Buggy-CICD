pipeline {
  agent any

  environment {
    IMAGE_TAG = 'latest'
  }

  stages {
    stage('Checkout Code') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: '*/Hemath']],
          userRemoteConfigs: [[
            url: 'https://github.com/Sedin-hemakumar/Buggy-CICD.git',
            credentialsId: 'Github password'
          ]]
        ])
      }
    }

    stage('Docker Login to ECR') {
      steps {
        withCredentials([
          string(credentialsId: 'AWS access key ID', variable: 'AWS_ACCESS_KEY_ID'),
          string(credentialsId: 'AWS secret access key', variable: 'AWS_SECRET_ACCESS_KEY'),
          string(credentialsId: 'AWS session token', variable: 'AWS_SESSION_TOKEN'),
          string(credentialsId: 'AWS_REGION', variable: 'AWS_REGION'),
          string(credentialsId: 'ECR_REGISTRY', variable: 'ECR_REGISTRY')
        ]) {
          sh '''
            aws ecr get-login-password --region "$AWS_REGION" | docker login --username AWS --password-stdin "$ECR_REGISTRY"
          '''
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        withCredentials([
          string(credentialsId: 'ECR_REGISTRY', variable: 'ECR_REGISTRY'),
          string(credentialsId: 'IMAGE_NAME', variable: 'IMAGE_NAME')
        ]) {
          sh '''
            docker build --no-cache -f Dockerfile.app -t ${IMAGE_NAME}:${IMAGE_TAG} .
            docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
          '''
        }
      }
    }

    stage('Push Image to ECR') {
      steps {
        withCredentials([
          string(credentialsId: 'ECR_REGISTRY', variable: 'ECR_REGISTRY'),
          string(credentialsId: 'IMAGE_NAME', variable: 'IMAGE_NAME')
        ]) {
          sh '''
            docker push ${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}
          '''
        }
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
