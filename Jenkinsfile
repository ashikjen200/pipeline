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
                 stage("Approval-deploy") {
                    steps {
                     // get user that has started the build
                  // first of all, notify the team Job started
                    slackSend (channel: "${params.SLACK_CHANNEL}", color: '#4286f4', message: "Deploy Approval: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})")
                script {
                    try {
                        timeout(time:30, unit:'MINUTES') {
                            env.APPROVE_PROD = input message: 'Deploy to Production', ok: 'Continue',
                                parameters: [choice(name: 'APPROVE_PROD', choices: 'YES\nNO', description: 'Deploy from STAGING to PRODUCTION?')]
                            if (env.APPROVE_PROD == 'YES'){
                                env.DPROD = true
                            } else {
                                env.DPROD = false
                            }
                        }
                    } catch (error) {
                        env.DPROD = true
                        echo 'Timeout has been reached! Deploy to PRODUCTION automatically activated'
                    }
                }
                      } // steps
                  } // stage
                 stage('Two') {
                 steps {
                    echo 'Hi complete'
                 }
                 }

              }
           post {
           success {
             slackSend (color: "good",
            channel: "${params.SLACK_CHANNEL}",
            message: "*SUCCESS:*\n *Job Name:* ${env.JOB_NAME}\nMore info at: ${env.BUILD_URL}\nMail sent to  DevOps Team")
                                   
           }
           failure {
             slackSend (color: "danger",
            channel: "${params.SLACK_CHANNEL}",
            message: "*FAILED:*\n*Job Name:* ${env.JOB_NAME}\n *Build No:* ${env.BUILD_NUMBER}\n *Stage Name:* ${FAILED_STAGE} has failed.\nMore info at: ${env.BUILD_URL}\nMail sent to DevOps Team")
               
           }
    }  
    
}
