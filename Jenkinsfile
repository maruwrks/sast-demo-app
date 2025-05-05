pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/your-username/sast-demo-app.git', branch: 'master'
            }
        }
        stage('SAST Analysis') {
            steps {
                sh 'bandit -f xml -o bandit-output.xml -r . || true'
                sh 'cat bandit-output.xml' // Menampilkan hasil bandit untuk pemeriksaan
            }
        }
    }
}
