node {
   def mvnHome
   stage('Prepare') {
      git 'https://github.com/jAnshula/spring-boot-demo.git'
      mvnHome = tool 'maven'
   }
   stage('Compile') {
      if (isUnix()) {
         sh "sudo ${mvnHome} -Dmaven.test.failure.ignore=true clean compile"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean compile/)
      }
   }
   stage('Code Review') {
   if (isUnix()) {
   sh "mvn -Dmaven.test.failure.ignore=true clean package"
   } else {
   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
   }
 }
   stage('Unit Testing') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "mvn -Dmaven.test.failure.ignore=true clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      if (isUnix()) {
         sh "mvn sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
      }
   }
   stage('Push the Artifacts to Nexus/Jfrog') {
      if (isUnix()) {
         sh "mvn -Dmaven.test.failure.ignore=true clean deploy"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean deploy/)
      }
   }
      stage('Push the Artifacts to Nexus/Jfrog') {
      if (isUnix()) {
         sh "mvn -Dmaven.test.failure.ignore=true clean deploy"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean deploy/)
      }
   }
   stage('Deploy') {
       sh 'curl -u admin:admin -T target/**.war "http://127.0.0.1:7080/manager/text/deploy?path=/web-demo&update=true"'
   }
   stage("Smoke Test"){
       sh "curl --retry-delay 10 --retry 5 http://127.0.0.1:7080/web-demo/users/1"
   } 
}
