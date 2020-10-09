pipeline {
    agent any
    environment {
        TOMCAT_HOST = 'servers_tomcat_1:8082'
        NEXUS_HOST = 'servers_nexus_1:8083'
        SONAR_HOST = 'servers_sonarqube_1:8084'
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