pipeline {
	agent any
	stages {
		stage('Build'){
		    steps{
		     bat 'mvn -B -DskipTests clean package' 
		}
		stage('Static Code Analysis'){
		    steps{
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
        stage('Deliver for development') {
            steps {
                bat """.\\jenkins\\scripts\\deliver.sh"""
            }
        }
	}
}
}