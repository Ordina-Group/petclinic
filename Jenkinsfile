
pipeline {
    agent any

    stages {
      stage('Build') { 
        steps {
            sh 'mvn --errors --update-snapshots install -DskipTests'
        }
      }
      
      stage('Test') { 
        steps {
            sh 'mvn --errors clean test'
        }
      }
      
      stage('Copy artifact to s3') {
        steps {
            sh 'echo ok'
        }
      }
   }
}
