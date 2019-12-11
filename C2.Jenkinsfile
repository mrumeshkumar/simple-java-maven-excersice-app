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
                    "Junit": {
                        bat 'mvn test'
                    },
                    "DB-Unit": {
                        echo 'DB Unit Test'
                    },
                    "Jesmine": {
                        echo 'Jesmine Test'
                    }
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
        
    }
}