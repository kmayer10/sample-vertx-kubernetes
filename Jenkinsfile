pipeline {
    agent {
		label 'ravi-paajee'
	}
	stages {
		stage('Checkout'){
			steps{
				git branch: 'openbatift', url: 'https://github.com/kmayer10/sample-vertx-kubernetes.git'
			}
		}
		stage('Package Application'){
			steps {
				bat 'mvn clean package -Dmaven.test.skip=true'
			}
		}
		stage('Build Docker Image'){
			steps {
				bat 'cd account-vertx-service && docker build -t docker.io/kulbhubatanmayer/node-app:v1.0 .'
			}	
		}
		stage('Scan Image using Trivy'){
			steps {
				bat 'trivy image --exit-code 0 --severity HIGH,CRITICAL docker.io/kulbhubatanmayer/node-app:v1.0'
			}	
		}
		stage('Pubat Docker Image to Registry'){
			steps {
				bat 'docker pubat docker.io/kulbhubatanmayer/node-app:v1.0'
			}	
		}
		stage('Activate myproject'){
			steps {
				bat 'oc project app'
			}	
		}
		stage('Delete Service'){
			steps {
				bat 'oc delete svc/node-app'
			}	
		}
		stage('Delete Deployment Configuration'){
			steps {
				bat 'oc delete dc/node-app'
			}	
		}
		stage('Redeploy App'){
			steps {
				bat 'oc new-app --docker-image="docker.io/kulbhubatanmayer/node-app:v1.0"'
			}	
		}
	}
}