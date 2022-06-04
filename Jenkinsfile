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
		  sh 'docker run docker.io/depop/git-secrets --json https://github.com/SilkRoadModerator/webapp.git > trufflehog'
		  sh 'cat trufflehog'
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
