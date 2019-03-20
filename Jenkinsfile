GITHUB_REPO="pibba"
GITHUB_PROJECT = "https://github.com/nerfeboi/" + GITHUB_REPO + ".git"

node{
   // Stages
   stage('Init'){
      if (env.BRANCH_NAME.startsWith('PR')) {
         echo "Branch Name: ${env.BRANCH_NAME}"
         echo "Change Branch Name: ${env.CHANGE_BRANCH}"
         //FINAL_BRANCH = env.CHANGE_BRANCH + "-" + env.BRANCH_NAME
         FINAL_BRANCH = env.CHANGE_BRANCH
      }else{
         FINAL_BRANCH = env.GIT_BRANCH
      }      
   }
   stage ("Retrieve all branch from github") {
      echo GITHUB_PROJECT
      git branch:"${FINAL_BRANCH}"//, url:GITHUB_PROJECT
      sh "echo Sonar Project Key: ${env.BRANCH_NAME}"
   }
   stage('Build') {
      sh "mvn -Dmaven.test.failure.ignore clean package"
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Code Quality - Sonarqube') {
      if (env.BRANCH_NAME.startsWith('PR')) {
         echo "Branch Name: ${env.BRANCH_NAME}"
         echo "Change Branch Name: ${env.CHANGE_BRANCH}"
         //FINAL_BRANCH = env.CHANGE_BRANCH + "-" + env.BRANCH_NAME
         FINAL_BRANCH = env.CHANGE_BRANCH
      }else{
         FINAL_BRANCH = env.GIT_BRANCH
      }
      echo "Final Branch Name: ${FINAL_BRANCH}"
      echo "Change ID: ${env.CHANGE_ID}"

       if (env.BRANCH_NAME.startsWith('PR')) {
            sh "mvn -Dsonar.projectKey='${FINAL_BRANCH}-sit-${env.BRANCH_NAME}' -Dsonar.projectName='${FINAL_BRANCH}-sit-${env.BRANCH_NAME}' sonar:sonar"
            sh "mvn -Dsonar.projectKey='${FINAL_BRANCH}-uat-${env.BRANCH_NAME}' -Dsonar.projectName='${FINAL_BRANCH}-uat-${env.BRANCH_NAME}' sonar:sonar"                                            
            sh "mvn -Dsonar.pullrequest.key=${env.CHANGE_ID} -Dsonar.pullrequest.branch='${FINAL_BRANCH}-sit-${env.BRANCH_NAME}' -Dsonar.projectKey='${FINAL_BRANCH}-sit-${env.BRANCH_NAME}' -Dsonar.projectName='${FINAL_BRANCH}-sit-${env.BRANCH_NAME}' sonar:sonar"
            sh "mvn -Dsonar.pullrequest.key=${env.CHANGE_ID} -Dsonar.pullrequest.branch='${FINAL_BRANCH}-uat-${env.BRANCH_NAME}' -Dsonar.projectKey='${FINAL_BRANCH}-uat-${env.BRANCH_NAME}' -Dsonar.projectName='${FINAL_BRANCH}-uat-${env.BRANCH_NAME}' sonar:sonar"                                                                  
       }else{
            sh "mvn -Dsonar.projectKey='${FINAL_BRANCH}-sit' -Dsonar.projectName='${FINAL_BRANCH}-sit' sonar:sonar"
            sh "mvn -Dsonar.projectKey='${FINAL_BRANCH}-uat' -Dsonar.projectName='${FINAL_BRANCH}-uat' sonar:sonar"                      
            sh "mvn -Dsonar.branch.longLivedBranches.regex='(branch|release|${FINAL_BRANCH}).*' -Dsonar.branch.name='${FINAL_BRANCH}-sit' -Dsonar.projectKey='${FINAL_BRANCH}-sit' sonar:sonar"
            sh "mvn -Dsonar.branch.longLivedBranches.regex='(branch|release|${FINAL_BRANCH}).*' -Dsonar.branch.name='${FINAL_BRANCH}-uat' -Dsonar.projectKey='${FINAL_BRANCH}-uat' sonar:sonar"                      
       }
   }
}
