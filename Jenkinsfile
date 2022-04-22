pipeline { 
    agent any
	tools { 
        maven 'Maven 3.0.5' 
        jdk 'jdk8' 
		}
	environment {
        def BUILDVERSION = sh(script: "echo `date +%s`", returnStdout: true).trim()
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
            when {
			branch 'master'
			steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sushant1305/devops_assignment.git']]])
				}
			
            }
			steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/feature/java_project']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sushant1305/devops_assignment.git']]])
				}
        }
		stage ('Build') {
            steps {
                sh 'mvn clean package' 
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
		stage('Deploy Approval: Staging') {
            steps {
				input 'Deploy to Staging ?'
				}
			}
		stage('Deploy to Staging') {
            steps {
			  sh "aws elasticbeanstalk create-application-version --application-name devops_assignment_staging --version-label devops_assignment_staging-source_${BUILDVERSION} --source-bundle S3Bucket=bits-devops-assignment,S3Key=devops_assignment.war"
			  sh "aws elasticbeanstalk update-environment --application-name devops_assignment_staging --environment-name Devopsassignmentstaging-env --version-label devops_assignment_staging-source_${BUILDVERSION}"
            }
		}
		stage('Deploy Approval: Prod') {
			when {
				branch 'master'
			}
            steps {
				input 'Deploy to Prod ?'
				}
			}
		stage('Deploy to Prod') {
			when {
				branch 'master'
			}
            steps {
			  sh "aws elasticbeanstalk create-application-version --application-name devops_assignment_production --version-label devops_assignment_staging-prod_${BUILDVERSION} --source-bundle S3Bucket=bits-devops-assignment,S3Key=devops_assignment.war"
			  sh "aws elasticbeanstalk update-environment --application-name devops_assignment_production --environment-name Devopsassignmentproduction-env --version-label devops_assignment_staging-prod_${BUILDVERSION}"
            }
		}
	}
	post {  
         
         failure {  
            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL of build: ${env.BUILD_URL}", cc: '', from: '', mimeType: 'text/html', replyTo: '', subject: "FAILED: ${currentBuild.currentResult}: Job ${env.JOB_NAME}", to: "sushant.mad@gmail.com";  
         } 
     }  
}
	