pipeline { 
    agent any  
    stages { 
        stage('Checkout') { 
            steps { 
               checkout([$class: 'GitSCM', branches: [[name: '*/feature/java_project']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sushant1305/devops_assignment.git']]])
            }
        }
		stage('Clean WS'){
				steps{
					cleanWs()
					}
			}
		}
	}