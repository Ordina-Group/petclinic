
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
          steps { 
              sh "aws ec2 run-instances --count 1 --image-id ami-04d5cc9b88f9d1d39 \
                  --instance-type t2.micro --key-name yp-test --security-group-ids sg-0518735c186e87a70 \
                  --subnet-id subnet-045f773dd295ba4ea --associate-public-ip-address \
                  --user-data file://aws/ec2-setup --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=test-bas}]' \
                  --iam-instance-profile Name=YPDeployRole"
          }
      }
   }
}
