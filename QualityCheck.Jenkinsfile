pipeline {
    agent any
    parameters {
        string(defaultValue: "EndToEnd", description: 'What environment?', name: 'testtype')
    }
    stages {
        stage('Browser-Test') {
	        when {
                    expression {params.testtype=="EndToEnd"}
	            }
            steps {
                parallel(
                    "FireFox": {echo 'FireFox Browser Test'},
                    "Chrome": {echo 'Chrome Browser Test'},
                    "Edge": {echo 'Edge Browser Test'},
                    "Safari": {echo 'Safari Browser Test'}
                )
            }
        }
        stage('Mobile-Test') {
	        when {
                    expression {params.testtype=="EndToEnd"}
	            }
            steps {
                parallel(
                    "Android": {echo 'Android testing'},
                    "IPhone": {echo 'IPhone testing'},
                    "Windows": {echo 'IPhone testing'}
                )
            }
        }
         stage('Integration-Test') {
           when {
                    expression {params.testtype=="Integration"}
	            }
            steps {
                parallel(
                    "DataService-Integration": {echo 'Data Service Integration Testing'},
                    "ServiceBus-Integration": {echo 'ServiceBus Integration Testing'},
                    "PaymentService-Integration": {echo 'Payment Service Integration Testing'}
                    )
            }
        }
        stage('Penetration-Test') {
            when {
                    expression {params.testtype=="Penetration"}
	            }
            steps {
                parallel(
                    "Penetration-Nessus": {echo 'Run Penetration test with Nessus'},
                    "Penetration-Burpsuite": {echo 'Run Penetration test with Burpsuite'}
                    )
            }
        }
         stage('Performance-Test') {
	         when {
                    expression {params.testtype=="Performance"}
	            }
            steps {
                parallel(
                    "Performance-Jmeter": {echo 'Run Performance test  with Jmeter'}
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