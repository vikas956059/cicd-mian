pipeline {

  environment {
    dockerimagename = "vikas956059/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'master', credentialsId: 'jenkinsgithub', url: 'https://github.com/vikas956059/cicd-mian.git'
      }
    }

    stage('Build image') {
      steps{
        script {
           dockerImage = docker.build("${dockerimagename}:${BUILD_NUMBER}")
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhublogin'
      }
      steps{
        script {
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
  }
}

