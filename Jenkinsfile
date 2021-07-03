pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/SKSCodeGitHUb/time5.git']]])
            }
        }
        stage('Clean Test') {
            steps {
                bat 'mvn clean test'
            }
        }
		
		stage('Clean Package') {
            steps {
                bat 'mvn clean package'
            }
        }
		
		stage('Deploy') {
            steps {
                bat "copy target/*.war D:/Tomcat/apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46/webapps"
			
            }
        }
		
		stage('Stop Tomcat') {
            steps {
                bat 'cd D:/Tomcat/apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46/bin'
				bat 'shutdown.bat'
            }
        }
		stage('Start Tomcat') {
            steps {
                bat 'cd D:/Tomcat/apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46/bin'
				bat 'start.bat'
            }
        }
    }
}