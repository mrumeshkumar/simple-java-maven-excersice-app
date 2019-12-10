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
		stage('upload artifect'){
			steps{
				archiveArtifacts '/target/*.jar'
			}
		}
        
    }
}