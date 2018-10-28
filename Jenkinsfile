pipeline {
  agent {
    docker {
      image 'gradle:4.3.0-jdk8-alpine'
    }
  }

  options {
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr:'5'))
  }

  triggers {
    pollSCM('H/15 * * * *')
  }

  stages {
    stage('Cleanup before build') {
      steps {
        cleanWs()
      }
    }

    stage('Checkout from Github') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'gradle build'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
    }
  }
}
