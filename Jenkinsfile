pipeline {

  environment {
    dockerimagename = "node"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: 'jenkinsgithub', url: 'https://github.com/vikas956059/cicd-mian.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build(dockerimagename)
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhublogin'
      }
      steps{
        script {
          docker.withRegistry('https://hub.docker.com/repositories/vikas956059', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }
  }
}
