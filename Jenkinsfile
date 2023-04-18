pipeline {

  environment {
    dockerimagename = "raja9985259937/firstimage"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Git checkout'){
      steps {
        git branch: 'main',
        url: 'https://github.com/kosurumuniraja/Springboot.git'
      }
    }

    /*stage('Checkout Source') {
      steps {
        git credentialsId: 'git-cred', url: 'https://github.com/kosurumuniraja/Springboot.git'
      }
    }*/

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'docker-cred'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          sh 'cd /var/lib/jenkins/workspace/hello-world'
          sh 'ls -l'
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
