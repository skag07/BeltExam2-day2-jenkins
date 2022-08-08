pipeline {
    agent any

    stages {
        
         stage('Quality Scan') {
            steps {
               
            sh "mvn -Dmaven.test.failure.ignore=true clean compile"
               
            sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=Onsite-Saeed_Alghamdi-B2D2 \
  -Dsonar.host.url=http://52.23.193.18 \
  -Dsonar.login=sqp_04fd739c888df0804e43dab72260b041c2174d42"

               
            }

            
        }
        
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/skag07/BeltExam2-day2-jenkins.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true package"

            }
            
             post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
                success {
                    
                    archiveArtifacts 'target/*.jar'
                }
            }
            
        }
        
       
    }
}