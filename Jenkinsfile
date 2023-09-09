pipeline {

  environment {
    registry = "sangeethapriya1998/flask"
    registry_mysql = "sangeethapriya1998/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/sangeethapriya1998/Docker-Project.git'
      }
    }
     
    stage('Build image') {
      steps{
        script {
          
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       script{
        withDockerRegistry(credentialsId: ' sangeethavenugopal', url: 'docker') {
        sh 'docker build -t "sangeethapriya1998/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
       
        }
        }
      }
   }
       stage('push mysql image') {
      steps{
        script {
         withDockerRegistry(credentialsId: ' sangeethavenugopal', url: 'docker') {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
