pipeline {
	agent any
	stages {
		stage('SCM') {
			steps {
				checkout scm
			}
		}
		stage('build') {
			agent {
				docker {
					image 'maven:3.6.0-jdk-8-alpine'
					reuseNode true
				}
			}
			steps {
				sh 'mvn clean compile'
			}
		}
		stage('test') {
			steps {
				sh 'mvn test'
			}
			post {
				always {
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		stage('checkstyle') { 
            		steps {
                		sh 'mvn checkstyle:checkstyle' 
            		}
            		post {
                		always {
                    			recordIssues enabledForFailure: true, tool: checkStyle()
                		}
	            	}
        	}
		stage('sonar'){
			steps{
				sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=projet1 \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqp_b222170a9172d500fd3d9b12d0bdc8115edd14a4'
			}
		}
	}
}
