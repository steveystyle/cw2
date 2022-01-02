pipeline {
  environment {
    registry = 'steveystyle/server-app'
    registryCredential = 'dockerhub'
    dockerImage = ''
    result = ''
    bNO = "${BUILD_NUMBER}.0"
    CI = 'true'
  }
  agent any
  stages {

    stage('Clone Git') {
      steps {
        git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/steveystyle/cw2.git'
      }
    }

    stage('Build Image') {
      steps {
        script {
          dockerImage = docker.build "${env.registry}:${env.bNo}"
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Build Test') {
      steps {
        script {
          dockerImage.inside {
            try {
              ans3 = fileExists file: 'serrver.js'
              echo"${ans3}"
              ans = sh(script: 'node serrver.js &', returnStdout: true)
              echo"${ans}"
            } catch (e) {
              echo "Caught: ${e}"
              currentBuild.result = 'failure'
            }
            try {
              ANS2 = sh(script: 'node server.js &', returnStatus: true)
              echo"${ANS2}"
              ANS4 = sh(script: 'node server.js &', returnStdout: true).trim()
              echo"${ANS4}"
            } catch (err) {
              echo "Caught: ${err}"
              currentBuild.result = 'failure'
            }
          }
        }
      }
    }
  }
}
