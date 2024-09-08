pipeline {
  agent any
  environment {
    MAVEN_HOME = tool name: 'maven', type: 'maven'
    USER_NAME = 'nkcharan'
    DOCKERHUB_CREDENTIALS = credentials('dockers')
  }
  stages {
    stage('Clone the Repository') {
      steps {
        git branch: 'health', credentialsId: 'git', url: 'https://github.com/charannk007/Health-Care-Staragile.git'
      }
    }

    stage('Build the Repository') {
      steps {
        script {
          echo "Maven Home: ${env.MAVEN_HOME}"
          sh "${MAVEN_HOME}/bin/mvn -version"
          sh "${MAVEN_HOME}/bin/mvn clean package"
        }
      }
    }

    stage('Build the Docker Image') {
      steps {
        sh 'docker build -t capstonehealth .'
      }
    }

    stage('Docker Login') {
      steps {
       script {
        withCredentials([usernamePassword(credentialsId: 'dockers', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
      }
    }
      }
    }
  }

  post {
    success {
      script {
        echo 'Pipeline succeeded! Image has been pushed to Docker Hub.'
      }
    }
  }
}
