node('ravi-paajee') {
	stage 'Checkout'
		git branch: 'openshift', url: 'https://github.com/kmayer10/sample-vertx-kubernetes.git'
	stage 'Package Application'
		when {
			branch 'openshift'
		}
		steps {
			sh 'mvn clean package -Dmaven.test.skip=true'
		}
	stage 'Build Docker Image'
		sh 'cd account-vertx-service && docker build -t docker.io/kulbhushanmayer/node-app:v1.0 .'
	stage 'Scan Image using Trivy'
		sh 'export TRIVY_VERSION=0.9.2 && rpm -ivh https://github.com/aquasecurity/trivy/releases/download/{TRIVY_VERSION}/trivy_{TRIVY_VERSION}_Linux-64bit.rpm && trivy image --exit-code 1 --severity HIGH,CRITICAL docker.io/kulbhushanmayer/node-app:v1.0'
	stage 'Push Docker Image to Registry'
		sh 'docker push docker.io/kulbhushanmayer/node-app:v1.0'
	stage 'Activate myproject'
		sh 'oc project app'
	stage 'Delete Service'
		sh 'oc delete svc/node-app'
	stage 'Delete Deployment Configuration'
		sh 'oc delete dc/node-app'
	stage 'Redeploy App'
		sh 'oc new-app --docker-image="docker.io/kulbhushanmayer/node-app:v1.0"'
	}
