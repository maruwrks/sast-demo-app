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
                    bandit -f sarif -o bandit-output.sarif -r . || true
                    echo "--- SARIF Output ---"
                    cat bandit-output.sarif || echo "SARIF not found!"
                    ls -lah
                '''
                recordIssues tools: [sarif(pattern: 'bandit-output.sarif')]
            }
        }
    }
}
