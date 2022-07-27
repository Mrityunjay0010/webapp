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
	    stage ("Build Stage") {
	      steps {
	sh"mvn clean package"
	      }
	    }
		  
	    stage ('Deploy-To-Tomcat') {
            steps {
sshagent(['Tomcat']) {
sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-11-19:/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
    }
stage ('Check-Git-Secrets') {
steps {
sh 'rm trufflehog || true'
sh 'docker run gesellix/trufflehog --json https://github.com/Mrityunjay0010/webapp.git > trufflehog'
sh 'cat trufflehog'
      }
    }
     stage ('Source Composition Analysis') {
      steps {
sh 'rmowasp* || true'
sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
sh 'chmod +x owasp-dependency-check.sh'
sh 'bash owasp-dependency-check.sh'
sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
     }
    }
		  stage ('DAST') {
      steps {
sshagent(['Tomcat']) {
sh 'ssh -o  StrictHostKeyChecking=no ubuntu@ip-172-31-46-11 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://http://65.1.130.191:8080/webapp/" || true'
        }
      }
    }
  }
}

	

