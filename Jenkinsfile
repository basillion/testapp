pipeline {
    agent any
    
    environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-basillion-id')
	}
   
    stages {
        stage('Build image') {
            steps {
                sh "docker build -t basillion/testapp ."
            }
        }
        
		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		
        stage('Push image in the hub') {
            steps {
                sh 'docker push basillion/testapp'
            }
        }
        
        stage('k8s deployment') {
            steps {
                sh "kubectl create deploy testapp --image basillion/testapp --replicas=4"
            }
        }
        stage('Expose deployment') {
            steps {
                sh "kubectl expose deploy testapp --port 80 --type LoadBalancer"
            }
        }
        
    }
    
	post {
		always {
			sh 'docker logout'
		}
	}
}

