pipeline {
	  agent any
	  tools {
	    maven "Maven"
	  }
	  stages {
	    stage ("Initialize") {
	      steps {
	sh'''
	                    echo "PATH = ${PATH}"
	                    echo "M2_HOME = ${M2_HOME}"
	            '''
	      }
	    }
		stage ('Check-Git-Secrets') {
           steps {
           sh 'rm trufflehog || true'
           sh 'docker run gesellix/trufflehog --json https://github.com/Mrityunjay0010/webapp.git > trufflehog'
           sh 'cat trufflehog'
      }
    }   
	    stage ("Build Stage") {
	      steps {
	sh"mvn clean package"
	      }
	    }
		stage ('Sonar-Qube') {
           steps {
           withSonarQubeEnv('Sonar') {
           sh 'mvn sonar:sonar'
        }
      }
    } 
	    stage ('Deploy-To-Tomcat') {
           steps {
           sshagent(['Tomcat']) {
           sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-11-19:/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
      }
	   stage ('DAST') {
      steps {
        sshagent(['tomcat']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@65.2.146.45 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.75.208.116:8080/webapp/" || true'
        }
      }
    }
  }
}
