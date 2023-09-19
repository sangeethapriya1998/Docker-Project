pipeline {

  environment {
    registry = "sangeethavenugopal/flask"
    registry_mysql = "sangeethavenugopal/mysql"
    dockerImage = ""
     dockerImage1 = ""
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
       script {
         dockerImage = docker.build registry_mysql + ":$BUILD_NUMBER"   
      }
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
          withKubeConfig([credentialsId: 'kube']) {
          sh 'kubectl apply -f frontend.yaml'
        }
      }
    }
    }
}
}
