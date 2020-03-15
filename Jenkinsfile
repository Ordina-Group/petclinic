
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
      
      stage('Publish Snapshot') {
        steps {
            sh 'echo ok'
        }
      }
   }
}
