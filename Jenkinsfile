pipeline {
  agent { label 'appServer' }
  environment {
    DOCKERHUB_CREDENTIALS = 'cybr-3120'
    IMAGE_NAME = 'stajose/chatapp'
  }

  stages {
    stage('Cloning Git') {
      steps { checkout scm }
    }



    stage('BUILD-AND-TAG') {
      agent { label 'appServer' }
      steps {
        script {
          echo "Building Docker image ${IMAGE_NAME}..."
          def app = docker.build("${IMAGE_NAME}")
          app.tag('latest', true)
          stash name: 'built-image-name', includes: ''
        }
      }
    }

    stage('POST-TO-DOCKERHUB') {
      agent { label 'appServer' }
      steps {
        script {
          echo "Pushing image ${IMAGE_NAME}:latest to Docker Hub"
          docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
            docker.image("${IMAGE_NAME}:latest").push()
          }
        }
      }
    }

    stage('Pull-image-server') {
      steps { sh 'echo Pulling image on server...' }
    }


  }
}