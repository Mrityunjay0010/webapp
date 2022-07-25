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
	      stage ('Deploy-To-Tomcat') {
            steps {
sshagent(['Tomcat']) {
sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-33-173/:/opt/tomcat/apache-tomcat-9.0.65/webapps/webapp.war'
              }      
           }       
    }
  }
}

        

