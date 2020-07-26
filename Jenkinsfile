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
		stage('Scan Image using Trivy'){
			steps {
				sh 'trivy image --exit-code 0 --format template --template "@junit.tpl" -o junit-report.xml --severity HIGH,CRITICAL docker.io/kulbhushanmayer/node-app:v1.0'
				junit allowEmptyResults: true, testResults: 'junit-report.xml'
			}	
		}
		stage('Push Docker Image to Registry'){
			steps {
				sh 'docker push docker.io/kulbhushanmayer/node-app:v1.0'
			}	
		}
		stage('Deploy App on DEV Cluster'){
			steps {
				sh 'oc project app'
				sh 'oc delete svc/node-app'
				sh 'oc delete dc/node-app'
				sh 'oc new-app --docker-image="docker.io/kulbhushanmayer/node-app:v1.0"'
			}	
		}
		stage('Release Deployment on UAT Cluster'){
			steps {
				input 'Would you like to Procced?'
				sh 'oc project uat-app'
				sh 'oc delete svc/node-app'
				sh 'oc delete dc/node-app'
				sh 'oc new-app --docker-image="docker.io/kulbhushanmayer/node-app:v1.0"'
			}	
		}
	}
}