pipeline { 
    agent any
	tools { 
        maven 'Maven 3.0.5' 
        jdk 'jdk8' 
		}
	
    stages {
		stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        
		stage('Checkout') { 
            steps { 
               checkout([$class: 'GitSCM', branches: [[name: '*/feature/java_project']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sushant1305/devops_assignment.git']]])
            }
        }
		stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean package' 
				}
			}
		stage('Deploy'){
				steps{
					sh 'scp target/devops_assignment.war pi@192.168.1.16:/home/pi/softwares/tomcat/apache-tomcat-8.5.78/webapps'
					}
			}
		stage('Clean WS'){
				steps{
					cleanWs()
					}
			}
		}
	}
	
	