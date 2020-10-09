pipeline {
    agent any
    environment {
        TOMCAT_HOST = 'servers_tomcat_1:8080'
        NEXUS_HOST = 'servers_nexus_1:8081'
        SONAR_HOST = 'servers_sonarqube_1:9000'
    }
    stages {
		stage('Compile') {
			steps {
                script {
                    compile.call()
                }
            }
        }
        stage('Sonar Verify Test') {
            steps {
                script {
                    sonar.call()
                }
            }
        }
        stage('Deploy Nexus') {
            steps {
                script {
                    nexus.call()
                }
            }
        }
        stage('Deploy Tomcat') {
            steps {
                script {
                    tomcat.call()
                }
            }
        }
    }
}