pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/maruwrks/sast-demo-app.git', branch: 'master'
            }
        }

        stage('Install Bandit') {
            steps {
                sh '''
                    python3 -m pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                sh '''
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                recordIssues(
                    tools: [bandit(pattern: 'bandit-output.xml')]
                )
                archiveArtifacts artifacts: 'bandit-output.xml', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
