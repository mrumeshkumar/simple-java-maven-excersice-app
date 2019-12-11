pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -B -DskipTests clean package' 
            }
        }
        stage('SonarQube Analysis') { 
            steps {
               // bat 'mvn sonar:sonar' 
               echo 'Build Completed succesfully'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
		stage('Upload Artifect'){
			steps{
				archiveArtifacts '/target/*.jar'
			}
			post{
			    success {
                        echo 'This will run only if successful'
                    }
			}
		}
    }
    post {
	    always {
	    	echo 'Build Completed succesfully'
	    }
	    failure {
	   		 echo 'Escalation email to Team'
  			 }
    }
}