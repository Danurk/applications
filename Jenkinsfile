pipeline {
    agent any
    
    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins-desafio', url: 'https://github.com/Danurk/applications']])
            }
        }
        
        
      
        
        stage ("App Dev") {
          when {
            branch 'dev'
          }
              steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
                sh "kubectl apply -f namespaces/dev.yaml"
                sh "kubectl apply -f appA/app-a.yaml -n dev"
                sh "kubectl apply -f appB/app-b.yaml -n dev"
                    
                }
              }
        }

        stage ("App Prod") {
          when {
            branch 'main'
          }
              steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '') {
                sh "kubectl apply -f namespaces/prod.yaml"
                sh "kubectl apply -f appA/app-a.yaml -n prod"
                sh "kubectl apply -f appB/app-b.yaml -n prod"
                    
                }
              }
        }
    }
}
