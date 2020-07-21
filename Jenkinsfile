pipeline {
         agent any
     parameters {
      string(name: 'SLACK_CHANNEL',
           description: 'Default Slack channel to send messages to',
           defaultValue: '#jenkin-sonar')
     } // parameters
	 

  stages {
                stage("Check requirement") {
               steps {
                     // get user that has started the build
                  // first of all, notify the team Job started
                       slackSend (color: "good",
                       channel: "${params.SLACK_CHANNEL}",	
                       message: "*STARTED:* Job: ${env.JOB_NAME} Build:${env.BUILD_NUMBER} has Started.\nMore info at: ${env.BUILD_URL}")
                      } // steps
                  } // stage
		  stage('checkout') {
			  
	          steps {
			 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/ashikjen200/pipeline.git']]])
                 }
		 }
                 stage('One') {
				
                 steps {
                     sh "sh sample.sh"
                 }
                 }
                 stage('Two') {
                 steps {
                    echo 'Hi complete'
                 }
                 }

              }
	    post {
            always {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }
}
