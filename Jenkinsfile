def urlSonar = ""

pipeline {
	agent any
	environment {
            SONAR_URL = "http://localhost"
            SONAR_PORT = "9000"
        }
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
			when {
				anyOf{branch 'sonar';branch 'master'}
			}
			steps{
				sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=projet1 -Dsonar.host.url=$SONAR_URL:$SONAR_PORT -Dsonar.login=sqp_b222170a9172d500fd3d9b12d0bdc8115edd14a4'
			}
		}
		
	}
}
