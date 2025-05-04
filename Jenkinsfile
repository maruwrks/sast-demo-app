pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/maruwrks/sast-demo-app.git', branch: 'master'
            }
        }
        
        stage('Setup Environment') {
            steps {
                sh '''
                    sudo apt-get update
                    sudo apt-get install -y python3 python3-venv
                    python3 -m venv venv
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
                    qualityGates: [[threshold: 1, type: 'TOTAL_HIGH', unstable: true]]
                )
            }
        }
    }
}
