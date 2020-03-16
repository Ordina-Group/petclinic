
pipeline {
    agent any

    stages {
      stage('Build') { 
        steps {
            sh 'mvn clean install'
        }
      }
      
      stage('Copy artifact to s3') {
        steps {
            sh 'aws s3 cp target/*jar s3://jworks-yp-releases/bas-petclinic.jar'
        }
      }

      stage('Deploy EC2 server') { 
          steps { 
              sh 'aws ec2 run-instances --count 1 --image-id ami-04d5cc9b88f9d1d39 \
                  --instance-type t2.micro --key-name yp-test --security-group-ids sg-0518735c186e87a70 \
                  --subnet-id subnet-045f773dd295ba4ea --associate-public-ip-address \
                  --user-data file//./aws/ec2-setup'
          }
      }
   }
}
