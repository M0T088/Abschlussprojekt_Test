//String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:9000'
    }
    stages {
		stage('docker-compose up') {
			steps {
				sh "docker-compose up --build -d"
          }
        }
        stage('Sonar Verify Test') {
            /*agent {
                docker {
                    image 'maven'
                    args '--network moto_net'
                }
            }*/
            steps {
                configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                    sleep (5)
                    sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean verify sonar:sonar'
                }
            }
        }
        stage('Deploy to Nexus Repo') {
            /*agent {
                docker {
                    image 'maven'
                    args '--network moto_net'
                }
            }*/
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean deploy -DskipTests -DdeployOnly'
                    }
                }
            }
        }
        /*stage('Deploy to Tomcat') {
            agent {
                docker {
                    image 'maven'
                    args '--network moto_net'
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'f3ef7a41-a468-41f2-8819-7a02ecf6050b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS tomcat7:deploy -DskipTests -DdeployOnly'
                    }
                }
            }
        }*/
        stage('Deploy War File to Tomcat') {
            steps {
				ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'deploy.yml', disableHostKeyChecking: true
                ansiblePlaybook inventory: 'inventory', colorized: true, installation: 'ansible2', playbook: 'remove.yml', disableHostKeyChecking: true
            }
        }
    }
}