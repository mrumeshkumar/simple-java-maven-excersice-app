pipeline {
   agent any

   stages {
      stage('Build') {
         steps {
            parallel(
              "C1-Reg": {
               build job: 'C1-Reg', propagate: true, wait: true
              },
              "C2-Billing": {
                build job: 'C2-Billing', propagate: true, wait: true
              },
              "C3-Coding": {
                build job: 'C3-Coding', propagate: true, wait: true
              },
              "C4-Eligibility": {
                build job: 'C4-Eligibility', propagate: true, wait: true
              }
            )
          }
      }
      stage ('Build-C1-Reg') {
            steps {
                build job: 'C1-Reg', propagate: true, wait: true
            }
        }
   }
}