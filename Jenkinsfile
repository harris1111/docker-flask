pipeline {

  agent none

  environment {
    DOCKER_IMAGE = "harris1111/flask-docker"
    registry = "harris1111/flask-docker" 
    registryCredential = 'harris1111' 
    dockerImage = '' 
  }

  stages {
    stage("Test") {
      agent {
          docker {
            image 'python:3.8-slim-buster'
            args '-u 0:0 -v /tmp:/root/.cache'
          }
      }
      steps {
        sh "pip install poetry"
        sh "poetry install"
        sh "poetry run pytest"
      }
    }
  stages { 
        stage('Cloning our Git') { 
            steps { 
                git 'https://github.com/YourGithubAccount/YourGithubRepository.git' 
            }
        } 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
  //   stage("build") {
  //     agent { node {label 'master'}}
  //     environment {
  //       DOCKER_TAG="${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"
  //     }
  //     steps {
  //       sh "sudo apt update"
  //       sh "sudo apt -V install gnupg2 pass"
  //       sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} . "
  //       sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
  //       sh "docker image ls | grep ${DOCKER_IMAGE}"
  //       withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
  //           sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
  //           sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
  //           sh "docker push ${DOCKER_IMAGE}:latest"
  //       }

  //       //clean to save disk
  //       sh "docker image rm ${DOCKER_IMAGE}:${DOCKER_TAG}"
  //       sh "docker image rm ${DOCKER_IMAGE}:latest"
  //     }
  //   }
  // }

  post {
    success {
      echo "SUCCESSFUL"
    }
    failure {
      echo "FAILED"
    }
  }
}
