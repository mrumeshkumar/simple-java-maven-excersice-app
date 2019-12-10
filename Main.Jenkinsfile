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
                build job: 'C1-Reg', propagate: true, wait: true
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
              "Azure-Deploy": { build job: 'C1-Reg', propagate: true, wait: true},
              "AWS-Deploy": {build job: 'C2-Billing', propagate: true, wait: true},
              "GCP-Deploy": { build job: 'C3-Coding', propagate: true, wait: true},
              "On-PremDataCenter": {build job: 'C4-Eligibility', propagate: true, wait: true}
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
                build job: 'C1-Reg', propagate: true, wait: true
            }
        }
   }
}