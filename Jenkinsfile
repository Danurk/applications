/*
pipeline {
  agent any
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
    disableConcurrentBuilds()
  }
  stages {
    stage('Hello') {
      steps {
        echo "hello"
      }
    }
    stage('for the fix branch') {
      when {
        branch "appAB"
      }
      steps {
        sh '''
          cat README.md
        '''
      }
    }
    stage('for the CD') {
      when {
        branch 'appCD'
      }
      steps {
        echo 'this only runs for appCD'
      }
    }
  }
}
*/

pipeline {
    agent any
    
    environment {
        registry = "551940803425.dkr.ecr.us-east-1.amazonaws.com/repo"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'abd03b63-9e13-437d-9bf9-9d421cc22029', url: 'https://github.com/Danurk/applications']]])
            }
        }
        
        
        stage ("Push into ECR") {
            steps {
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 551940803425.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 551940803425.dkr.ecr.us-east-1.amazonaws.com/repo:latest"
            }   
        }
        
        
        stage ("Application AB") {
          when {
            branch 'appAB'
          }
              steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
                sh "kubectl apply -f appAB/app-a.yaml -f appAB/app-b.yaml -f appAB/ingress.yaml"
                    
                }
              }
        }

        stage ("Application CD") {
          when {
            branch 'appCD'
          }
              steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
                sh "kubectl apply -f appCD/app-c.yaml -f appCD/app-d.yaml -f appCD/ingress2.yaml"
                    
                }
              }
        }
    }
}
