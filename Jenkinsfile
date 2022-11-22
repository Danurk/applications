pipeline {
  agent {label "linux"}
  options {
    buildDiscarder logRotator(artifactDaysTokeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
    disableConcurrentBuilds()
  }
  stages {
    stage('Hello') {
      steps {
        echo "hello"
      }
    }
  }  	
}