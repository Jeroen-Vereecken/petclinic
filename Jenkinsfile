
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
            sh 'aws s3 cp target/*jar s3://jworks-yp-releases/jeroen-petclinic.jar'
        }
      }

      stage('Deploy EC2 server') { 
          steps { 
              sh "aws ec2 run-instances --count 1 --image-id ami-0ec1ba09723e5bfac \
                  --instance-type t2.micro --key-name Jeroen-Demo-Key --security-group-ids jeroen-demo-sg \
                  --subnet-id subnet-a37495ca --associate-public-ip-address \
                  --user-data file://aws/ec2-setup --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=demo-jeroen}]' \
                  --iam-instance-profile Name=YPDeployRole --region eu-central-1"
          }
      }
   }
}
