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
         stage('Configure-SubNet-QA') {
	        when {
                    expression {
                        params.environment=="QA"
                    }
	            }
            steps {
                echo 'Configure QA SubNet'
            }
        }
         stage('Configure-SubNet-Stage') {
            when {
                    expression {
                        params.environment=="stage"
                    }
	            }
            steps {
                echo 'Configure stage SubNet'
            }
        }
        stage('Configure-SubNet-Production') {
            when {
                    expression {
                        params.environment=="production"
                    }
	            }
            steps {
                echo 'Configure Production SubNet'
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
                    catchError() {
                          bat "exit 1"
                     }
                }
        }
        stage('Deploy - Production') {
	        when {
                    expression {
                        params.environment=="production"
                    }
	            }
            steps {
               // input message: 'Are You ready for Production Deployment ? (Click "Proceed" to continue)'
                bat """.\\jenkins\\scripts\\production.sh"""
            }
        }
    }
    post {
	    always {
	    	echo 'Build Completed succesfully. Build Pipeline: ${currentBuild.fullDisplayName}'
             logstashSend failBuild: false, maxLines: 10000
	       // mail to: 'mrumeshkumar@hotmail.com',subject: "Build Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}"
	    }
	    failure {
	   		  echo 'Build Failed ! Initiate RollBack'
              build job: 'DeploymentRollback', propagate: true, wait: true ,parameters: [string(name: 'InfraProvider', value: "azure")]
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