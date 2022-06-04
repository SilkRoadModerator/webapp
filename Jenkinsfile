pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('lets began with Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    
    stage ('Bitch its time to Build') {
      steps {
      sh 'mvn clean package'
                
       }
      }
  
    stage('fuckers now we Deploy to Tomcat'){
	    steps {
	  sh 'scp target/*.war /home/jenkins/prod/apache-tomcat-8.5.79/webapps/webapp.war'
	  }
    }
    
   }
}
