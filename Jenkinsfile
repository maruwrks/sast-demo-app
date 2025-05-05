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
                sh 'bandit -f xml -o bandit-output.xml -r . || echo "Bandit scan completed with warnings"'
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
            post {
                always {
                    sh 'ls -lah bandit-output.xml'
                }
            }
        }
    }
}
