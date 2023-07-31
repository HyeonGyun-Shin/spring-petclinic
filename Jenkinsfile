pipeline {
	agent any
	tools {
		maven "Maven"
	}
	
	stages {
		stage('Build') {
			steps {
				sh 'mvn package'
			}
		}
	}
}
