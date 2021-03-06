pipeline {
    agent any
    environment { 
    	GITUrl = 'https://github.com/SKSCodeGitHUb/time5.git'
        TomcatPath = 'D:/Tomcat/apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46'
        Tomcat_Login = 'jenkins'
        Tomcat_URL = 'http://localhost:8081/'
        Context_Path = 'Web'
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
               	deploy adapters: [tomcat9(credentialsId: 'ebf5fe32-90d2-4763-8d6b-a6cca5d64870', path: '', url: 'http://localhost:8081/')], contextPath: Context_Path, war: '**/*.war'
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