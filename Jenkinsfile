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
stage('Docker Login') {
            steps {
                script {
                    // Your Docker Hub or container registry credentials
                    def registryUrl = 'https:///hub.docker.com'
                    def registryCredentialsId = 'sangeethavenugopal1998'

                    // Retrieve Docker registry username and password from Jenkins credentials
                    def registryCredentials = credentials(sangeethavenugopal1998)

                    // Docker login using --password-stdin
                    sh """
                    echo \\"${registryCredentials.sangeethavenugopal}\\n${registryCredentials.password}\\" | docker login -u \\"${registryCredentials.username}\\" --password-stdin ${registryUrl}
                      """
                }
            }
}
    stage('Push Flask Image') {
      steps{
        script {
          withDockerRegistry([credentialsId:"sangeethavenugopal1998", url: "" ]) {
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
          sh 'docker login -p(credentialsId:"sangeethavenugopal1998", url: "")'
         
      }
   }
      
    stage('Push MySQL Image') {
      steps{
        script {
          withDockerRegistry([credentialsId:"sangeethavenugopal1998", url: "" ]) {
            dockerImage.push("registry_mysql")
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
