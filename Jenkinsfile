def registry = 'https://trialq5nhpo.jfrog.io'
pipeline {
    agent any
    environment {
	PATH = "/opt/maven/bin:$PATH"
    }	
    stages {
	stage("Build") {
		steps {
		  echo "---------Build Started-----------"
		  	sh 'mvn clean package -Dmaven.test.skip=true'
		  echo "---------Build Ended-------------"
		}
        }
	stage("Test") {
                steps {
                  echo "---------Test Started-----------"
                        sh 'mvn surefire-report:report'
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
	stage("Jar Publish") {
            steps {
                script {
                    echo '-------Jar Publish Started---------'
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "jfrog-cred"
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                    def uploadSpec = """{
                        "files": [
                          {
                              "pattern": "jarstaging/(*)",
                              "target": "lokesh-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}"
                              "exclusions": [ "*.shal", "*.md5"]
                          }
                        ]
                    }"""
                    def buildInfo = server.upload(uploadSpec)
                    uploadSpec.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo "------JarPublish Ended--------"
                }
                
            }
            
        }

    }
}


