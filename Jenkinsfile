pipeline {
         agent any
         stages {
		  stage('checkout') {
				 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/ashikjen200/pipeline.git']]])
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
}
