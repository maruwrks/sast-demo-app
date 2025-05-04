pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/maruwrks/sast-demo-app.git', branch: 'master'
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate  # Aktifkan virtual environment lagi!
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                recordIssues(
                    tools: [bandit(pattern: 'bandit-output.xml')]
                )
            }
        }
    }
}
