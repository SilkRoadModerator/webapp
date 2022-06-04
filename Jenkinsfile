pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
	  stage ('Check Git Secrets') {
	  steps {
		  sh 'rm trufflehog || true'
		  sh 'docker run docker.io/dxa4481/trufflehog https://github.com/SilkRoadModerator/webapp.git > trufflehog'
		  sh 'cat trufflehog'
	  }
  }
	  
	  stage ('Source Composition Analysis') {
		  steps {
	
			  sh 'rm owasp* || true'
			  sh 'wget https://raw.githubusercontent.com/SilkRoadModerator/webapp/main/owasp-dependency-check.sh'
			  sh 'chmod -x owasp-dependency-check.sh'
			  sh 'bash owasp-dependency-check.sh'
		  }
	  }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
                
       }
      }
  
    stage('Deploy to Tomcat'){
	    steps {
	  sh 'scp target/*.war /home/jenkins/prod/apache-tomcat-8.5.79/webapps/webapp.war'
	  }
    }
    
   }
}
