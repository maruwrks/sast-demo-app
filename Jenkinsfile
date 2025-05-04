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
                sh 'pip install bandit'
            }
        }
        stage('SAST Analysis') {
            steps {
                sh 'bandit -f xml -o bandit-output.xml -r . || true'  // Menjalankan bandit dan output ke XML
                // Menggunakan plugin recordIssues untuk menampilkan hasilnya di Jenkins
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
