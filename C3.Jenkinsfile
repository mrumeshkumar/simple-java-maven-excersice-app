pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Sonar Scan') { 
            steps {
                bat 'mvn sonar:sonar' 
            }
        }
        stage('Unit-Test') {
            steps {
                parallel(
                    "Junit": {bat 'mvn test'},
                    "DB-Unit": {echo 'DB Unit Test'},
                    "Jesmine": {echo 'Jesmine Test'}
                )
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
		}
        
         post {
	    always {
	    	echo 'Build Completed succesfully. Build Pipeline: ${currentBuild.fullDisplayName}'
             logstashSend failBuild: false, maxLines: 10000
	       // mail to: 'mrumeshkumar@hotmail.com',subject: "Build Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}"
	    }
	    failure {
	   		 echo 'Build Failed !'
        //mail to: 'mrumeshkumar@hotmail.com',subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",body: "Something is wrong with ${env.BUILD_URL}"
   			 }
        success {
            echo 'This will run only if successful'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
    }
}