pipeline {
    agent any
    environment{
        PATH= "/opt/maven/bin:$PATH"
    }

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/lokeshbomma/sparkjava-war-example.git', branch: 'master'
            }
        }
         stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
