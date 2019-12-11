pipeline {
   agent any

   stages {
      stage('Build') {
           parallel {
               stage('C1-Reg') {
                   steps {
                    build job: 'C1-Reg', propagate: true, wait: true
                   }
               }
               stage('C2-Billing') {
                   steps {
                      build job: 'C2-Billing', propagate: true, wait: true
                   }
               }
               stage('C3-Coding') {
                   steps {
                     build job: 'C3-Coding', propagate: true, wait: true
                   }
               }
                stage('C4-Eligibility') {
                   steps {
                     build job: 'C4-Eligibility', propagate: true, wait: true
                   }
               }
            }
    //       steps {
    //         parallel(
    //          "C1-Reg": { build job: 'C1-Reg', propagate: true, wait: true},
    //          "C2-Billing": { build job: 'C2-Billing', propagate: true, wait: true},
    //          "C3-Coding": {build job: 'C3-Coding', propagate: true, wait: true},
    //          "C4-Eligibility": {build job: 'C4-Eligibility', propagate: true, wait: true}
    //        )
    //      }
      }
       stage ('Deploy-QA') {
            steps {
                 parallel(
                        "AzureDeploy": { build job: 'AzureDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "QA")]},
                        "AWSDeploy": { build job: 'AWSDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "QA")]},
                        "GCPDeploy": {build job: 'GCPDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "QA")]},
                        "OnPremDeploy": {build job: 'OnPremDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "QA")]}
                     )   
                
            }
        }
       stage('Test-QA') {
          steps {
            parallel(
              "EndToEnd": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "EndToEnd")]},
              "Integration": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Integration")]},
              "Penetration": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Penetration")]},
              "Performance": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Performance")]}
            )
          }
      }
        stage('Deploy-Stage') {
         steps {
            parallel(
                        "AzureDeploy": { build job: 'AzureDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "stage")]},
                        "AWSDeploy": { build job: 'AWSDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "stage")]},
                        "GCPDeploy": {build job: 'GCPDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "stage")]},
                        "OnPremDeploy": {build job: 'OnPremDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "stage")]}
                     )   

          }
      }
       stage('Test-Stage') {
         steps {
            parallel(
              "EndToEnd": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "EndToEnd")]},
              "Integration": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Integration")]},
              "Penetration": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Penetration")]},
              "Performance": {build job: 'QualityCheck', propagate: true, wait: true,parameters: [string(name: 'testtype', value: "Performance")]}
            )
          }
      }
       stage ('Approval') {
            steps {
                input message: 'Are all department GREEN to go Deployment ? (Click "Proceed" to continue)'
            }
        }
      stage ('Production') {
            steps {
                parallel(
                        "AzureDeploy": { build job: 'AzureDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "production")]},
                        "AWSDeploy": { build job: 'AWSDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "production")]},
                        "GCPDeploy": {build job: 'GCPDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "production")]},
                        "OnPremDeploy": {build job: 'OnPremDeploy', propagate: true, wait: true ,parameters: [string(name: 'environment', value: "production")]}
                     )   
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