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
    
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
                
       }
      }
  
    stage('Deploy to Tomcat'){
	  sh "scp target/*.war /home/jenkins/prod/apache-tomcat-8.5.79/webapps
	  }
    
   }
}
