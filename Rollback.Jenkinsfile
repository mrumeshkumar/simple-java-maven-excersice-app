pipeline {
    agent any
    parameters {
        string(defaultValue: "azure", description: 'What environment?', name: 'InfraProvider')
    }
    stages {
        stage('Azure Deployment Rollback') {
             when {
                    expression {
                        params.InfraProvider=="azure"
                    }
	            }
            stages {
                stage("Rollback") {
                    steps {
                               echo 'Rollback successful'
                    }
                }
            }
            post {
                success {
                    echo 'Deployment Rollback successful'
                }
            }
        }
        stage('AWS Deployment Rollback') {
             when {
                    expression {
                        params.InfraProvider=="aws"
                    }
	            }
            stages {
                stage("Rollback") {
                    steps {
                               echo 'Rollback successful'
                    }
                }
            }
            post {
                success {
                    echo 'Deployment Rollback successful'
                }
            }
        }
        stage('GCP Deployment Rollback') {
             when {
                    expression {
                        params.InfraProvider=="gcp"
                    }
	            }
            stages {
                stage("Rollback") {
                    steps {
                               echo 'Rollback successful'
                    }
                }
            }
            post {
                success {
                    echo 'Deployment Rollback successful'
                }
            }
        }
        stage('OnPrem Deployment Rollback') {
             when {
                    expression {
                        params.InfraProvider=="ownprem"
                    }
	            }
            stages {
                stage("Rollback") {
                    steps {
                               echo 'Rollback successful'
                    }
                }
            }
            post {
                success {
                    echo 'Deployment Rollback successful'
                }
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