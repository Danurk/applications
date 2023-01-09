pipeline {
    agent any
    
    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins-desafio', url: 'https://github.com/Danurk/applications']])
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
