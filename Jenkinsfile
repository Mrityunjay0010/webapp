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
	sh "mvn clean package"
	      }
	    }
		 stage ('Deploy-To-Tomcat') {
             steps {
              sshagent(['Tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ip-172.31.5.44:/opt/tomcat/apache-tomcat-10.0.8/webapps/webapps.war'
              }      
           }       
	 }
	}
}

