
pipeline {
    agent any
    
    environment { 
      s3CFReleaseBucket = "jworks-cf-releases"
      stackName = "bas-petclinic"
    }

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

      stage('Copy cloudformation to S3') { 
        steps {
          sh "aws s3 cp cloudformation/petclinic.yml s3://${s3CFReleaseBucket}/${stackName}.yml"
        }
      }

      stage('Deploy EC2 server with cloudformation') { 
          steps { 
            sh "aws cloudformation create-stack --region eu-west-1 \
                --stack-name ${stackName} \
                --capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_IAM\
                --template-url https://${s3CFReleaseBucket}.s3-eu-west-1.amazonaws.com/${stackName}.yml \
                --tags Key=Environment,Value=Dev Key=Owner,Value=JWorks"
            sh "aws cloudformation wait stack-create-complete --stack-name ${stackName}"
          }
      }
   }
}
