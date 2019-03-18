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
                   sh "mvn -Dsonar.branch.longLivedBranches.regex='(branch|release|${env['GIT_BRANCH']}).*' -Dsonar.branch.name='${env['GIT_BRANCH']}-sit' -Dsonar.projectKey='${env['GIT_BRANCH']}-sit' sonar:sonar"
               }              
           }
           steps{
               withSonarQubeEnv('Sonarqube') {
                   sh "mvn -Dsonar.branch.longLivedBranches.regex='(branch|release|${env['GIT_BRANCH']}).*' -Dsonar.branch.name='${env['GIT_BRANCH']}-uat' -Dsonar.projectKey='${env['GIT_BRANCH']}-uat' sonar:sonar"
               }              
           }          
       }
   }
}
