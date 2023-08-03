pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git 'https://github.com/your-username/auto-sec-scan-pipeline.git'
            }
        }
        
        stage('Static Code Analysis') {
            steps {
                // Install Bandit for Python code security analysis
                sh 'pip install bandit'
                
                // Run Bandit for Python code security analysis
                sh 'bandit -r secureapp'
            }
        }
        
        stage('OWASP ZAP Scan') {
            steps {
                // Start OWASP ZAP in daemon mode
                sh 'docker run -d -p 9090:8080 --name zap owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080 -config api.disablekey=true'
                
                // Wait for ZAP to start (adjust the sleep time as needed)
                sh 'sleep 30'
                
                // Run ZAP baseline scan on your web application
                sh 'docker exec zap zap-baseline.py -t http://13.233.49.98'
                
                // Stop and remove the ZAP container
                sh 'docker stop zap && docker rm zap'
            }
        }
        
        stage('Docker Image Scanning') {
            steps {
                // Install Trivy for Docker image scanning
                sh 'wget https://github.com/aquasecurity/trivy/releases/download/v0.20.0/trivy_0.20.0_Linux-64bit.tar.gz'
                sh 'tar zxvf trivy_0.20.0_Linux-64bit.tar.gz'
                sh 'sudo mv trivy /usr/local/bin/'
                
                // Scan Docker images with Trivy
                sh 'trivy image your-docker-image:tag'
            }
        }
    }
}
