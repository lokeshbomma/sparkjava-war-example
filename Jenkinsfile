pipeline {
    agent any
    environment {
	PATH = "/opt/maven/bin:$PATH"
    }	
    stages {
	stage("Build") {
		steps {
		  echo "---------Build Started-----------"
		  	sh 'mvn clean package Dmaven.test.skip=true'
		  echo "---------Build Ended-------------"
		}
        }
	stage("Test") {
                steps {
                  echo "---------Test Started-----------"
                        sh 'mvn sunfire-report:report'
                  echo "---------Test Ended-------------"
                }
        }

        stage('SonarQube Analysis') {
                environment {
                    scannerHome = tool 'saidemy-sonar-scanner'
                }
		steps {
	            withSonarQubeEnv('saidemy-sonarqube-server') {
                	sh "${scannerHome}/bin/sonar-scanner"
          	    }
       		}	
        }
    }
}


