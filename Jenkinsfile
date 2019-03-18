pipeline {
   agent any
   stages{
       stage('Build') {
           steps{
                 echo 'test'
                 sh "mvn -Dmaven.test.failure.ignore clean package"
           }
       }
       stage('Results') {
           steps{
                junit '**/target/surefire-reports/TEST-*.xml'
                archive 'target/*.jar'
           }
       }
       stage("Code Quality - Sonarqube"){
           steps{
               withSonarQubeEnv('Sonarqube') {
                   sh "mvn -Dsonar.projectKey=pibfe sonar:sonar"
               }              
           }
       }
   }
}