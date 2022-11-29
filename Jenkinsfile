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
