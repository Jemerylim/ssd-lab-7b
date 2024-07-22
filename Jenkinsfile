pipeline {
 agent none
 stages {
  stage('Integration UI Test') {
   parallel {
    stage('Deploy') {
     agent any
     steps {
                           sh 'chmod +x ./jenkins/scripts/deploy.sh'
      sh 'dos2unix ./jenkins/scripts/deploy.sh'
      sh './jenkins/scripts/deploy.sh'
      input message: 'Finished using the web site? (Click "Proceed" to continue)'
                           sh 'chmod +x ./jenkins/scripts/kill.sh'
      sh 'dos2unix ./jenkins/scripts/kill.sh'
      sh './jenkins/scripts/kill.sh'
     }
    }
    stage('Headless Browser Test') {
     agent {
      docker {
       image 'maven' 
       args '-u root' 
      }
     }
     steps {
      sh 'mvn -B -DskipTests clean package'
      sh 'mvn test'
     }
     post {
      always {
       junit 'target/surefire-reports/*.xml'
      }
     }
    }
   }
  }
 }
}