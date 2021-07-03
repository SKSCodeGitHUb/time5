pipeline {
    agent any
    environment { 
    	GITUrl = 'https://github.com/SKSCodeGitHUb/time5.git'
        TomcatPath = 'D:/Tomcat/apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46'
        Tomcat_Login = 'jenkins'
        Tomcat_URL = 'http://localhost:8081/'
        Context_Path = 'TimeTracker'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: GITUrl
            }
        }
 		stage("Build") {
			steps {
				parallel(
				      cleaning: {
					bat "mvn clean"
				      },
				      testing: {
					bat "mvn test"
				      },
				      packageing: {
					bat "mvn -Dmaven.test.failure.ignore=true package"
				      }
				)
			}
			post {
			    always { 
           			 echo 'Build step executed'
       			 }
                success {
                	echo 'Build Successful'
                    archiveArtifacts 'web/target/*.war'        
                }   
            }       
		}
		stage('Tomcat Deployment') {
            steps {
               	deploy adapters: [tomcat9(credentialsId: Tomcat_Login, path: '', url: Tomcat_URL)], contextPath: Context_Path, onFailure: false, war: '**/*.war'
            }

            post {
                  always { 
           			 echo 'Deployment step executed'
       			 }
                success {
                	echo 'Deployment Successful'
                }
                
            }
        }
    }
}