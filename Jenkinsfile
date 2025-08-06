pipeline {
    agent any
    tools{
        nodejs 'NodeJS'
    }
    stages {
        stage('Hello, I am Mohamed Eltelawi') {
            steps {
                echo 'This is GitOps Pipeline practice'
            }
        }
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/moeltelawi/Jenkins-ArgoCD-GitOps.git'
            }
        }
        stage('Install node dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('npm audit fix') {
            steps {
                sh 'npm audit fix'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                echo 'Build Docker Image'
                docker.build("eltelawi/gitops-crash:latest")
                }
            }
        }
        stage('Image Scan using Trivy'){
			steps {
				//sh 'trivy --severity HIGH,CRITICAL --no-progress image --format table -o trivy-scan-report.txt ${DOCKER_HUB_REPO}:latest'
				sh 'trivy --severity HIGH,CRITICAL --skip-update --no-progress image --format table -o trivy-scan-report.txt eltelawi/gitops-crash:latest'
			}
		}
		stage('Push Image to Docker Hub') {
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'Docker-Hub', passwordVariable: 'DHpass', usernameVariable: 'DHuser')]) {
                sh '''
                echo "$DHpass" | docker login -u "$DHuser" --password-stdin
                docker push eltelawi/gitops-crash:latest
            '''
}
                
            }
        }
    }
}
