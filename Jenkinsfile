pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = 'ASIASJCHX6HESLMIQDMZ'
    AWS_SECRET_ACCESS_KEY = 'ZjwwC02aqKIWC8v1sygFck4rgSa3a28JnIdL3HbV'
    AWS_SESSION_TOKEN = 'IQoJb3JpZ2luX2VjEF8aCmFwLXNvdXRoLTEiRjBEAiA+z7FCSL2jA/N0o6hXFOezKYTMJ6WA9bD5Bhz2Bd9NPgIgJcFl9+0uV3BHEOW1JO0UdooRPR6JOdqugzL6pQOxW4EqpQMIiP//////////ARAAGgwxNTY5MTY3NzMzMjEiDCkR3LHvfe5LsBh72yr5AnXEk0I7tcKpN0voMPK+d7HTl3O2/iZyrYamP7Q8xuI9l0rVBeu5VYE7nvLvpKJYckzpiOZ5WaBjqii3Xz1C/T00hTMFR8GHOKoa6huIyznC+6UzQrLY1cV0F1Sg4R/am7YVsN8rBEftwZCj11W7XWSCWyLU63/295En4BWrxhAaTv+1gt9t5ZMSV2Zkp6X6JNYnt2RhaSp+UIOsr0rjZLcIr4wf2X2cuPVmbpW0mUlLr54yNvZeyNj+NcfQPZvb+xla9H9ptVrWcUPxXsbOvjHURPV6Fgfa7Yl/ztemN7lb4KWwVSADRDB0D/nGaH1DGPZnEDSlxQL0ClgDlofj3RyUMFwCT5XWMafqAo+nQYTuk38SK495HibZaD0YP/yVUC2egTaM+cVaqhhqpgQH4fUmXd6QYpEgTZ3zV7q7/C2B/oJy/wyuZf+Z2a/GZSSJczaCofimrX70MHHctVLQjGKaFVVxxuZ8iFdzYolI7nv5PmoaLNegULTWMLG2nMQGOqcBWtJYD56/eomg3jiBcJKYCb6/kqpIo6fznrqbA6foq2CyWYCZTCBOvyt0SL29GGKzcUuQq+b2ZIxCBuLFE/Oq69ZQl6PQvXQXnZTUNVavleGn6SjMIZniZp6MfnevIfrcxNRYWJRL6hsCgM/z5SwFmj8a94udea6MiFrh97akHBx5S4lGDH6AyyMaZVEqdmizlEop4QJEtnasZcBiFKCrkD7i3NIy/mo='
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
