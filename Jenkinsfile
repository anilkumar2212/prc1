pipeline {
    agent any
     environment {
        registry = "195051713942.dkr.ecr.ap-northeast-1.amazonaws.com/jenkins"
    }
   
    stages {
          stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anilkumar2212/prc1.git'
            }
        }
           stage('Building image') {
             steps{
                  script {
                   dockerImage = docker.build registry
                   }
      }
           }
    
            stage('Pushing to ECR') {
             steps{  
                  script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh 'aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 195051713942.dkr.ecr.ap-northeast-1.amazonaws.com'
     sh 'docker push 195051713942.dkr.ecr.ap-northeast-1.amazonaws.com/jenkins:latest'
}

}
                  }
            }
             stage('stop previous containers') {
               steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
            stage('Docker Run') {
              steps{
                   script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 195051713942.dkr.ecr.ap-northeast-1.amazonaws.com/jenkins:latest'     
      }
    }
        }
    }
  }
