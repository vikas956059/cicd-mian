pipeline {

  agent any

  environment {
    dockerimagename = "vikas956059/nodeapp"
    registryCredential = 'dockerhublogin'
  }

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'master',
            credentialsId: 'jenkinsgithub',
            url: 'https://github.com/vikas956059/cicd-mian.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${dockerimagename}:${BUILD_NUMBER}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            dockerImage.push()
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Rolling Update on EKS using kubectl') {
      steps {
        script {
          sh """
            echo '🔁 Updating deployment with new image...'
            kubectl set image deployment/nodeapp nodeapp=${dockerimagename}:${BUILD_NUMBER} --record
            kubectl rollout status deployment/nodeapp
            echo '✅ Rolling update completed.'
          """
        }
      }
    }
  }

}
