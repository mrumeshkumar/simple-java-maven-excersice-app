pipeline {
    agent any
    parameters {
        string(defaultValue: "TEST", description: 'What environment?', name: 'environment')
    }
    stages {
        stage('Get Base Image') {
            steps {
                    echo 'Get Base Image From artifactory'
                }
        }
         stage('Configure-SubNet') {
	        when {
                    expression {
                        params.environment=="QA"
                    }
	            }
            steps {
                echo 'Configure QA SubNet'
            }
            when {
                    expression {
                        params.environment=="stage"
                    }
	            }
            steps {
                echo 'Configure stage SubNet'
            }
            when {
                    expression {
                        params.environment=="production"
                    }
	            }
            steps {
                echo 'Configure production SubNet'
            }
        }
         stage('Deploy- QA') {
	        when {
                    expression {
                        params.environment=="QA"
                    }
	            }
            steps {
                bat """.\\jenkins\\scripts\\deliver.sh"""
            }
        }
        stage('Deploy - Staging') {
             when {
                    expression {
                        params.environment=="stage"
                    }
	            }
            steps {
                    echo './deploy staging'
                    echo './run-smoke-tests'
                }
        }
        stage('Deploy for production') {
	        when {
                    expression {
                        params.environment=="production"
                    }
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