pipeline {
  agent any

  environment {
        DOCKER_IMAGE_NAME = 'simple-node-app'
        DOCKER_CONTAINER_NAME = 'express-params-example'
        NODEJS_HOME = ''
        DOCKERFILE_PATH = '/'  // Dockerfile is in the root directory
  }

  stages {
    stage('checkout') {
      steps {
          checkout scm
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          // Build the Docker image from the specified Dockerfile
          docker.build("-f ${DOCKERFILE_PATH} -t ${DOCKER_IMAGE_NAME} .")
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          // Stop and remove the existing container if it exists
          sh "docker stop ${DOCKER_CONTAINER_NAME} || true"
          sh "docker rm ${DOCKER_CONTAINER_NAME} || true"

          // Run the Docker container
          sh "docker run -d --name ${DOCKER_CONTAINER_NAME} -p 3000:3000 ${DOCKER_IMAGE_NAME}"
        }
      }
    }
    post {
        success {
            echo "Docker image built successfully: ${DOCKER_IMAGE_NAME}"
            echo "Docker container running: ${DOCKER_CONTAINER_NAME}"
        }
    }
  }
}
