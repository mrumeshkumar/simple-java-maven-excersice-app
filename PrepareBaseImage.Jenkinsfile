pipeline {
    agent any
    parameters {
        string(defaultValue: "azure", description: 'What environment?', name: 'InfraProvider')
    }
    stages {
        stage('Prepare Azure Base Image') {
             when {
                    expression {
                        params.InfraProvider=="azure"
                    }
	            }
            stages {
                stage("Get Base Image") {
                    steps {
                               echo 'Get Base Image From artifactory'
                    }
                }
                stage("Setup Firewall Rule") {
                    steps {
                            echo 'Setup Firewall Rule'
                    }
                }
                stage("Install Jboss Web Server") {
                    steps {
                            echo 'Install Jboss Web Server'
                    }
                }
                stage("Install Redis Server") {
                    steps {
                            echo 'Install Redis Server'
                    }
                 }
                stage("Test Image") {
                    steps {
                            echo 'Test Image'
                        }
                }
                 stage("Uplod Image on Artifactory") {
                    steps {
                            echo 'Install Redis Server'
                    }
                 }
            }
            post {
                success {
                    echo 'Azure Base Image Created successful'
                }
            }
            
        }
         stage('Prepare AWS Base Image') {
             when {
                    expression {
                        params.InfraProvider=="aws"
                    }
	            }
            stages {
                stage("Get AWS Base Image") {
                    steps {
                               echo 'Get AWS Base Image From artifactory'
                        }
                    }
                stage("Setup Firewall Rule") {
                    steps {
                            echo 'Setup Firewall Rule'
                        }
                    }
                stage("Install Jboss Web Server") {
                    steps {
                            echo 'Install Jboss Web Server'
                        }
                    }
                stage("Install Redis Server") {
                    steps {
                            echo 'Install Redis Server'
                        }
                    }
                stage("Test Image") {
                    steps {
                            echo 'Test Image'
                        }
                }
                stage("Uplod Image on Artifactory") {
                    steps {
                            echo 'Install Redis Server'
                    }
                 }
            }
            
        }
         stage('Prepare GCP Base Image') {
             when {
                    expression {
                        params.InfraProvider=="gcp"
                    }
	            }
            stages {
                stage("Get GCP Base Image") {
                    steps {
                               echo 'Get AWS Base Image From artifactory'
                        }
                    }
                stage("Setup Firewall Rule") {
                    steps {
                            echo 'Setup Firewall Rule'
                        }
                    }
                stage("Install Jboss Web Server") {
                    steps {
                            echo 'Install Jboss Web Server'
                        }
                    }
                stage("Install Redis Server") {
                    steps {
                            echo 'Install Redis Server'
                        }
                    }
                stage("Test Image") {
                    steps {
                            echo 'Test Image'
                        }
                }   
                stage("Uplod Image on Artifactory") {
                    steps {
                            echo 'Install Redis Server'
                    }
                 }
            }
            
        }
        stage('Prepare OnPrem Base Image') {
             when {
                    expression {
                        params.InfraProvider=="onprem"
                    }
	            }
            stages {
                stage("Get GCP Base Image") {
                    steps {
                               echo 'Get AWS Base Image From artifactory'
                        }
                    }
                stage("Setup Firewall Rule") {
                    steps {
                            echo 'Setup Firewall Rule'
                        }
                    }
                stage("Install Jboss Web Server") {
                    steps {
                            echo 'Install Jboss Web Server'
                        }
                    }
                stage("Install Redis Server") {
                    steps {
                            echo 'Install Redis Server'
                        }
                    }
                stage("Test Image") {
                    steps {
                            echo 'Test Image'
                        }
                }
                stage("Uplod Image on Artifactory") {
                    steps {
                            echo 'Install Redis Server'
                    }
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