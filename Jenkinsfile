pipeline {

  environment {
    registry = "sangeethavenugopal/flask"
    registry_mysql = "sangeethavenugopal/mysql"
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
   
       stage('Push Flask Image') {
      steps{
           sh "docker login -u sangeethavenugopal -p Sangeetha@1998 " 
      }
        }
      
    stage('Push Flask Image1') {
      steps{
            sh " docker push 'docker.io/sangeethavenugopal/flask:$BUILD_NUMBER'"
        }
      }
    
   stage('Build mysql image') {
     steps{
         dockerImage = docker.build registry + ":$BUILD_NUMBER"
           
      }
   }
      
    stage('Push MySQL Image') {
      steps{
        script {
            sh " docker push 'docker.io/sangeethavenugopal/mysql:$BUILD_NUMBER'"
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
