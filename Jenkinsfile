pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/akshaybhu/FlaskTest.git'
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                    # Remove existing venv if present
                    rm -rf .venv || true
                    
                    # Create fresh virtual environment
                    python3 -m venv .venv
                    
                    # Activate virtual environment and install dependencies
                    . .venv/bin/activate
                    python3 -m pip install --upgrade pip
                    python3 -m pip install -r requirements.txt
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    . .venv/bin/activate
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    . .venv/bin/activate
                    python3 app.py
                '''
            }
        }
    }
    
    post {
        always {
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                    subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                    to: 'Akshay.thebest@yahoo.co.in'
        }
    }
}
