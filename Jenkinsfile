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
                // Membuat dan mengaktifkan virtual environment, lalu install Bandit
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                // Jalankan Bandit dengan output SARIF (untuk kompatibilitas dengan Jenkins)
                sh '''
                    . venv/bin/activate
                    bandit -f sarif -o bandit-output.sarif -r . || true
                '''

                // Rekam hasil analisis Bandit menggunakan SARIF parser
                recordIssues tools: [sarif(pattern: 'bandit-output.sarif')]
            }
        }
    }
}
