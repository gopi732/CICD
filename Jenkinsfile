pipeline {
  agent any
  stages { 
    stage('CleanUp WorkSpace & Git Checkout') {
      steps {
          // Clean before build
          cleanWs()
          // We need to explicitly checkout from SCM here
          checkout scm
      }
    }
    stage ("Testing & SonarQube analysis") {
      environment {
	       scannerHome = tool 'SonarQube Scanner'
      }
      steps {
         withSonarQubeEnv('Sonar') {
            sh "mvn clean verify install sonar:sonar -Dsonar.projectKey=Work \
		 -Dsonar.java.coveragePlugin=jacoco \
                 -Dsonar.jacoco.reportPaths=target/jacoco.exec \
    	         -Dsonar.junit.reportsPaths=target/surefire-reports"
         }
      }
      post {
         success {
            junit 'target/surefire-reports/**/*.xml' 
         }
      }
    }
    stage ('Archive artifacts') {
        steps {
           archiveArtifacts artifacts: 'target/*.war'
        }
    }	  
  }
}
