pipeline {
    agent any
    environment {
        TOMCAT_HOST = 'tomcat:8080'
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:9000'
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
                    tomcatdeploy.call()
                }
            }
        }
    }
}