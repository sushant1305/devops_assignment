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
        stage('Deploy') {
            steps {
			  sh "/home/pi/.local/bin/aws configure set region $AWS_DEFAULT_REGION"
              sh "/home/pi/.local/bin/aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID"  
              sh "/home/pi/.local/bin/aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY"
              sh "/home/pi/.local/bin/aws s3 cp target/devops_assignment.war s3://devopsassignment"
            }
		}
	}
}

	
	