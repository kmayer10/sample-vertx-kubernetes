pipeline {
    agent {
		label 'ravi-paajee'
	}
	stages {
		stage('Package Application'){
			steps {
				sh 'mvn clean package -Dmaven.test.skip=true'
			}
		}
		stage('Build Docker Image'){
			steps {
				sh 'cd account-vertx-service && docker build -t docker.io/kulbhushanmayer/node-app:v1.0 .'
			}	
		}
		//stage('Scan Image using Trivy'){
			//steps {
				//sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL docker.io/kulbhushanmayer/node-app:v1.0'
			//}	
		//}
		stage('Push Docker Image to Registry'){
			steps {
				sh 'docker push docker.io/kulbhushanmayer/node-app:v1.0'
			}	
		}
		stage('Activate myproject'){
			steps {
				sh 'oc project app'
			}	
		}
		stage('Delete Service'){
			steps {
				sh 'oc delete svc/node-app'
			}	
		}
		stage('Delete Deployment Configuration'){
			steps {
				sh 'oc delete dc/node-app'
			}	
		}
		stage('Redeploy App'){
			steps {
				sh 'oc new-app --docker-image="docker.io/kulbhushanmayer/node-app:v1.0"'
			}	
		}
	}
}