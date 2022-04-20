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
        stage('Publish') {
            steps {
			  sh "aws configure set region $AWS_DEFAULT_REGION"
              sh "aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID"  
              sh "aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY"
              sh "aws s3 cp target/devops_assignment.war s3://bits-devops-assignment"
            }
		}
		stage('Stage Deploy Approval') {
            steps {
				input 'Deploy to Staging ?'
				}
			}
		stage('Deploy to Staging') {
            steps {
			  sh "aws elasticbeanstalk create-application-version --application-name devops_assignment_staging --version-label devops_assignment_staging-source_${BUILD_NUMBER} --source-bundle S3Bucket=bits-devops-assignment,S3Key=devops_assignment.war"
			  sh "aws elasticbeanstalk update-environment --application-name devops_assignment_staging --environment-name Devopsassignmentstaging-env --version-label devops_assignment_staging-source_${BUILD_NUMBER}"
            }
		}
		
		stage('Deploy to Prod') {
            steps {
			  sh "aws elasticbeanstalk create-application-version --application-name devops_assignment_production --version-label devops_assignment_staging-prod_${BUILD_NUMBER} --source-bundle S3Bucket=bits-devops-assignment,S3Key=devops_assignment.war"
			  sh "aws elasticbeanstalk update-environment --application-name devops_assignment_production --environment-name Devopsassignmentproduction-env --version-label devops_assignment_staging-prod_${BUILD_NUMBER}"
            }
		}
	}
}
	