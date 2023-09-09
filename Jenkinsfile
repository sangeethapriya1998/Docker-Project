pipeline {

  environment {
    registry = "sangeethapriya1998/flask"
    registry_mysql = "10.138.0.3:5001/sangeethapriya1998/mysql"
    dockerImage = ""
    DOCKER_CREDENTIALS = credentials('sangeethavenugopal')
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/sangeethapriya1998/Docker-Project.git'
      }
    }
      stage('Build and Push Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/', 'sangeethavenugopal') {
                        // Build and push Docker image here
                    }
                }
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
          docker.withRegistry( "" ) {
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
       sh 'docker build -t "sangeethapriya1998/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "sangeethapriya1998/mysql:$BUILD_NUMBER"'
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
