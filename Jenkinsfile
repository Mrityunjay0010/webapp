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
	     sshagent(['Tomcat']) {
            sh "scp -o StrictHostKeyChecking=no target/*.war admin@13.234.116.168/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war"

           } 
	  }
    }
}
  


        

