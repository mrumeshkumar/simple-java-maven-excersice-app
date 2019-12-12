pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -B -DskipTests clean package' 
            }
            post{
			    success {
                    script {
                        currentBuild.result = 'SUCCESS'
                        }
                    }
                failure {
                     script {
                        currentBuild.result = 'FAILURE'
                        }
	   			    }   
        }
        stage('SonarQube Analysis') { 
            steps {
               // bat 'mvn sonar:sonar' 
               echo 'Build Completed succesfully'
            }
            post{
			    success {
                    script {
                        currentBuild.result = 'SUCCESS'
                        }
                    }                
                failure {
                     script {
                        currentBuild.result = 'FAILURE'
                        }
			        }   
        }
        stage('Test') {
            steps {
                bat 'mvn test'
                echo "RESULT: ${currentBuild.result}"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
			    success {
                    script {
                        currentBuild.result = 'SUCCESS'
                        }
                    }
                failure {
                     script {
                        currentBuild.result = 'FAILURE'
                        }
                    }
                }
		stage('Upload Artifect'){
            when{
                 expression {
                       currentBuild.result == 'SUCCESS'
                    } 
             }
			steps{
				archiveArtifacts '/target/*.jar'
			}
			post{
			    success {
                    script {
                          currentBuild.result = 'SUCCESS'
                         }
                    } 
                failure {
                    script {
                          currentBuild.result = 'FAILURE'
                         }
			        }
		        }
        stage('Notify'){
             when{
                 expression {
                       currentBuild.result == 'SUCCESS'
                    } 
             }
			steps{
				echo 'Notify To Teams.'
			}
		}
         stage('Escalate'){
            when{
                 expression {
                       currentBuild.result == 'FAILURE'
                    } 
             }
			steps{
				echo 'Escalation email to Team'
			}
		}
    }
    post {
	    always {
	    	echo 'Build Completed succesfully'
            echo "RESULT: ${currentBuild.result}"
	    }
	    failure {
	   		 echo 'Escalation email to Team'
  			 }
    }
}