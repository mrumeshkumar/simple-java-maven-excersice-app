pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -B -DskipTests clean package' 
            }
        }
        stage('SonarQube analysis') { 
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
		stage('Upload artifect'){
			steps{
				archiveArtifacts '/target/*.jar'
			}
		}
        stage('Deliver for development') {
	        when {
	                branch 'development'
	            }
            steps {
                bat """.\\jenkins\\scripts\\deliver.sh"""
            }
        }
        stage('Deploy - Staging') {
            steps {
                    echo './deploy staging'
                    echo './run-smoke-tests'
                }
        }
        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Deploy for production') {
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
	        mail to: 'mrumeshkumar@hotmail.com',
	             subject: "Build Pipeline: ${currentBuild.fullDisplayName}",
	             body: "Something is wrong with ${env.BUILD_URL}"
	    }
	    failure {
        mail to: 'mrumeshkumar@hotmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
   			 }
    }
}