pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('saeed-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('Saeed-aws-secret-access-key')
        ARTIFACT_NAME = 'belt2d2.jar'
        AWS_S3_BUCKET = 'saeed-belt2d2-artifacts-123456'
        AWS_EB_APP_NAME = 'Saeed-sample-application-b2d2'
        AWS_EB_ENVIRONMENT = 'Saeedsampleapplicationb2d2-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
    } 
    stages {

        
        
         stage('Quality Scan') {
            steps {
               
            sh "mvn -Dmaven.test.failure.ignore=true clean compile"

            sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=Onsite-Saeed_Alghamdi-B2D2 \
  -Dsonar.host.url=http://52.23.193.18 \
  -Dsonar.login=sqp_51bf35e07572a01c166a857e75696f47b7a2abe6"
               
      
               
            }

            
        }
        
        stage('Build') {
            steps {
                
                sh "mvn compile"

                
            }
            
             post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
                success {
                    
                    archiveArtifacts 'target/*.jar'
                    sh 'aws s3 cp ./target/**.jar s3://$AWS_S3_BUCKET/$ARTIFACT_NAME'
                }
            }
            
        }
        
       stage('Publish') {
            steps {
                sh 'aws configure set region us-east-1'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
            }
            post {
                success {
                    echo 'success Jenkinsfile'
                   
                }
            }
        }
    }
}
