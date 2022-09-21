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
		  stage ('Sonar-Qube') {
           steps {
           withSonarQubeEnv('Sonar') {
           sh 'mvn sonar:sonar'
        }
      }
    } 
	    stage ("Build Stage") {
	      steps {
	sh"mvn clean package"
	      }
	    }
		
	    stage ('Deploy-To-Tomcat') {
           steps {
           sshagent(['Tomcat']) {
           sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-26-187:/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
      }
	stage ('DAST') {
      steps {
        sshagent(['tomcat']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@ip-172-31-18-93:"docker run -t owasp/zap2docker-stable zap-baseline.py -t http://65.1.130.191:8080/webapp/" || true'
        }
      }
    }   
  }
}
