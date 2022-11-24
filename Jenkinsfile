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
	sh "mvn clean package"
	      }
	    }
	   stage ('Deploy-To-Tomcat') {
             steps {
              sshagent(['Tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172-31-7-7:/opt/tomcat/webapps/webapp.war'
              }      
           }       
      }
    }
}
