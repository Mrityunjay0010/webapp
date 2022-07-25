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
	                    echo "MAVEN_HOME = ${MAVEN_HOME}"
	            '''
	      }
	    }
	    stage ("Build Stage") {
	      steps {
	sh"mvn clean package"
	      }
	    }
		  stage('Deploy to Tomcat'){
			  steps {
	     sshagent(['Tomcat']) {
            sh "scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-46-45:/opt/tomcat/apache-tomcat-9.0.65/webapps/opt/tomcat/apache-tomcat-9.0.65/webapps/"
            } 
	  }
	  }
	  }
    }

  


        

