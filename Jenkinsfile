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
		}
        stage('Push To Development') {
	        when {
	                branch 'development'
	            }
            steps {
                bat """.\\jenkins\\scripts\\deliver.sh"""
            }
        }
        stage('Push To Staging') {
            steps {
                    echo './deploy staging'
                    echo './run-smoke-tests'
                }
        }
        stage('Sanity Check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Push To Production') {
	        when {
	                branch 'production'
	            }
            steps {
                input message: 'Are You ready for Production Deployment ? (Click "Proceed" to continue)'
                bat """.\\jenkins\\scripts\\production.sh"""
            }
        }
    }
    post {
	    always {
	    	echo 'Build Completed succesfully'
	       // mail to: 'mrumeshkumar@hotmail.com',subject: "Build Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}"
	    }
	    failure {
	   		 echo 'Build Failed !'
        //mail to: 'mrumeshkumar@hotmail.com',subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",body: "Something is wrong with ${env.BUILD_URL}"
   			 }
    }
}