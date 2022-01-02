node {
  
  def app
    
  withEnv(['CI=true','REGISTRY="steveystyle/server-app"','REGISTRY_CREDENTIAL=dockerhub','B_NO=BUILD_NUMBER + ".0"']){
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
      app = docker.build(REGISTRY)
    }

    stage('Test image') {
      app.inside {
        sh 'node serrver.js &'
      }
    }

    stage('Push image') {
      docker.withRegistry('', REGISTRY_CREDENTIAL) {
            app.push(B_No)
            app.push("latest")
        }
    }
}
  
  post{ 
    success{
      withKubeConfig([credentialsId: 'mykubeconfig']) {
        sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
        sh 'chmod u+x ./kubectl'
        sh "./kubectl set image deployments/server-app server-app=REGISTRY:B_No"
      }
      sh 'docker rmi REGISTRY:B_No'
      sh 'docker rmi app'
    }
    failure{
        sh 'docker rmi REGISTRY:B_No'
        sh 'docker rmi app'
    }
  }
}
