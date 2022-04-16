pipeline { 
    agent any
	tools { 
        maven 'Maven 3.0.5' 
        jdk 'jdk8' 
    }
	stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
    stages { 
        stage('Checkout') { 
            steps { 
               checkout([$class: 'GitSCM', branches: [[name: '*/feature/java_project']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sushant1305/devops_assignment.git']]])
            }
        }
		stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean package' 
            }
		stage('Clean WS'){
				steps{
					cleanWs()
					}
			}
		}
	}
	

    
    stages {
        

        stage ('Build') {
            steps {
                echo 'This is a minimal pipeline.'
            }