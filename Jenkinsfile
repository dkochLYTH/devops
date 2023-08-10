node {
   def mvnHome
   stage('Prepare') {
      git url: 'https://github.com/dkochLYTH/devops.git', branch: 'develop'
      mvnHome = tool 'Maven'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" sonar:sonar -Dsonar.projectKey=devops -Dsonar.projectName='devops'/)
      }
   }
   stage('Deploy') {
       bat(/ for \/f "delims=" %%a in ('dir \/s \/b *.war') do set "name=%%a" curl -u jenkins:jenkins -T %name% "http://localhost:8080/manager/text/deploy?path=/devops&update=true" /)

   }
   stage("Smoke Test"){
       bat "curl --retry-delay 10 --retry 5 http://localhost:8080/devops"
   }
}
