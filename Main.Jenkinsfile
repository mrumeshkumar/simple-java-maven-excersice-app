pipeline {
   agent any

   stages {
      stage('Build') {
         steps {
            parallel(
              "C1-Reg": { build job: 'C1-Reg', propagate: true, wait: true},
              "C2-Billing": { build job: 'C2-Billing', propagate: true, wait: true},
              "C3-Coding": {build job: 'C3-Coding', propagate: true, wait: true},
              "C4-Eligibility": {build job: 'C4-Eligibility', propagate: true, wait: true}
            )
          }
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
                "End-To-End": {build job: 'C1-Reg', propagate: true, wait: true},
                "Integration": {build job: 'C2-Billing', propagate: true, wait: true},
                "Panetration": {build job: 'C3-Coding', propagate: true, wait: true},
                "Performance": {build job: 'C4-Eligibility', propagate: true, wait: true}
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
              "End-To-End": {build job: 'C1-Reg', propagate: true, wait: true},
              "Integration": {build job: 'C2-Billing', propagate: true, wait: true},
              "Penetration": {build job: 'C3-Coding', propagate: true, wait: true},
              "Performance": {build job: 'C4-Eligibility', propagate: true, wait: true}
            )
          }
      }
       stage ('Approval') {
            steps {
                input message: 'Are You ready for Production Deployment ? (Click "Proceed" to continue)'
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
}