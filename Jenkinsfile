pipeline {
    agent any
    environment { 
    	GITUrl = 'https://github.com/SKSCodeGitHUb/time5.git'
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
					bat "copy web/target/*.war D:\Tomcat\apache-tomcat-9.0.46-windows-x64/apache-tomcat-9.0.46/webapps"
                }   
            }       
		}
    }
}