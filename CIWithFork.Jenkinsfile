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
                bat 'mvn sonar:sonar' 
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
	       // mail to: 'mrumeshkumar@hotmail.com',subject: "Build Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}"
	    }
	    failure {
	   		 echo 'Escaltion email to Team'
  			 }
    }
}