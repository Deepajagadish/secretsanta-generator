pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    environment{
        /*SCANNER_HOME= tool 'sonar-scanner'*/
	    APP_NAME = "santa123"
            RELEASE = "1.0.0"
            DOCKER_USER = "deepajagadish"
            DOCKER_PASS = "docker-cred"
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    } 

    stages {
	stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }
        stage('git-checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Deepajagadish/secretsanta-generator.git'
            }
        }

        stage('Code-Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        
        stage('Unit Tests') {
            steps {
               sh "mvn test"
            }
        }
	/*stage('trivy fs scan') {
            steps {
               sh "trivy fs --format table -o trivy-fs-report.html . "
            }
        } */
        
	/* stage('OWASP Dependency Check') {
            steps {
               dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '/dependency-check-report.xml'
            }
        } 


        stage('Sonar Analysis') {
            steps {
               withSonarQubeEnv('sonar'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Santa \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Santa '''
               }
            }
        } */

		 
        stage('Code-Build') {
            steps {
               sh "mvn clean package"
            }
        }

         stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker-cred') {
                    sh "docker build -t  ${${IMAGE_NAME}}" + ":" + "${IMAGE_TAG} . "
                 }
               }
            }
        }

        /*stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker-cred') {
                    sh "docker tag ${IMAGE_TAG}"
                    sh "docker push ${${IMAGE_NAME}}" + ":" + "${IMAGE_TAG}"
                 }
               }
            }
        }*/
	/*stage('Docker Image Scan') {
            steps {
               sh "trivy image deepajagadish/santa123:latest "
            }
        } 
	stage('Docker run-container') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker-cred') {
                    sh "docker run -d -p 8082:8080 deepajagadish/santa123:latest"
                 }
               }
            }
        }*/
	

    }
}
	    
        
        	 
        
	
        
         /* post {
            always {
                emailext (
                    subject: "Pipeline Status: ${BUILD_NUMBER}",
                    body: '''<html>
                                <body>
                                    <p>Build Status: ${BUILD_STATUS}</p>
                                    <p>Build Number: ${BUILD_NUMBER}</p>
                                    <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                                </body>
                            </html>''',
                    to: 'jaiswaladi246@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html'
                )
            }
        } */
		
		

    

