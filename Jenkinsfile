pipeline {
  agent any

  environment {
    IMAGE_NAME = "anandsbhavik12/jenkins-demo-app"
    IMAGE_TAG = "latest"
  }

  stages {
    stage('Checkout Code'){
      steps {
        checkout scm
      }
    }

    stage('Build Docker image') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME:$IMAGE_TAG .
        '''
      }
    }
  }
}

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
          '''
        }
      }
    }

    stage('Push Image') {
      steps {
        sh '''
          docker push $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
          docker rm -f jenkins-demo-app || true
          docker run -d \
            --name jenkins-demo-app \
            -p 5000:5000 \
            $IMAGE_NAME:$IMAGE_TAG
          '''
      }
    }
        
