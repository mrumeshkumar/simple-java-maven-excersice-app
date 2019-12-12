pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -B -DskipTests clean package' 
                echo "RESULT: ${currentBuild.currentResult}"
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
                echo "RESULT: ${currentBuild.currentResult}"
                
                catchError(currentBuildResult: 'SUCCESS', currentStageResult: 'FAILURE') {
                    bat "exit 1"
                }
            }
            post {  // 'stage 3'
                failure {
                    echo "... at least one failed"
                }
                success {
                    echo "Success!"
                }
            }
        }
		stage('Upload Artifect'){
            when{
                 expression {
                       currentBuild.currentResult == 'SUCCESS'
                    } 
             }
			steps{
				archiveArtifacts '/target/*.jar'
			}
        }
        stage('Notify'){
             when{
                 expression {
                       currentBuild.currentResult == 'SUCCESS'
                    } 
             }
			steps{
				echo 'Notify To Teams.'
			}
		}
         stage('Escalate'){
            when{
                 expression {
                       currentBuild.currentResult == 'FAILURE'
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