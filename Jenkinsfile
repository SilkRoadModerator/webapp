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
	  
	 stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }     
    	  
	  stage ('Source Composition Analysis') {
		  steps {
	
			  sh 'rm owasp* || true'
			  sh 'wget "https://raw.githubusercontent.com/SilkRoadModerator/webapp/main/OWASP_Dependency_Check.sh" '
			  sh 'chmod -x OWASP_Dependency_Check.sh'
			  sh 'bash OWASP_Dependency_Check.sh'
			  sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
