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
                // Membuat virtual environment di dalam workspace Jenkins
                sh 'python3 -m venv venv'  
                
                // Mengaktifkan virtual environment dan menginstal bandit
                sh 'source venv/bin/activate && pip install bandit'
            }
        }
        stage('SAST Analysis') {
            steps {
                // Mengaktifkan virtual environment dan menjalankan analisis Bandit
                sh 'source venv/bin/activate && bandit -f xml -o bandit-output.xml -r . || true'
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
