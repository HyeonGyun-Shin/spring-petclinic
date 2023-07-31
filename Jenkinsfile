pipeline {
	agent any
	tools {
		maven "Maven"
	}

        stages {
                stage('Git Clone') {
                        steps {
                                script {
                                        git branch: 'main',
                                                credentialsId: 'HyeonGyun-Shin',
                                                url: 'https://github.com/HyeonGyun-Shin/spring-petclinic
                                        
                                }
                        }
                }
        }
	stages {
		stage('Build') {
			steps {
				sh 'mvn package'
			}
		}
	}
}
