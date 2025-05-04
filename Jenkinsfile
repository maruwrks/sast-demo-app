pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/maruwrks/sast-demo-app.git', branch: 'master'
            }
        }
        
        stage('Setup Python') {
            steps {
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install bandit
                '''
            }
        }
        
        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                recordIssues(
                    tools: [banditParser(pattern: 'bandit-output.xml')],
                )
            }
        }
    }
}
