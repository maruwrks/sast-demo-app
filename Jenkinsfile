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
                    bandit -f xml -o bandit-output.xml -r . || true
                    echo "--- XML Output ---"
                    cat bandit-output.xml || echo "XML not found!"
                    ls -lah
                '''
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
