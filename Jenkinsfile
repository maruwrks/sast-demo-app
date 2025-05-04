pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/maruwrks/sast-demo-app.git', branch: 'master'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'  // Membuat virtual environment
                sh 'source venv/bin/activate && pip install bandit'  // Instal bandit di venv
            }
        }
        stage('SAST Analysis') {
            steps {
                sh 'source venv/bin/activate && bandit -f xml -o bandit-output.xml -r . || true'  // Jalankan bandit di venv
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
