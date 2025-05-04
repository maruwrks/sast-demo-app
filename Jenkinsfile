pipeline {
    agent {
        docker {
            image 'python:3.9-slim'
            args '--user root'
        }
    }
    
    stages {
        stage('SAST Analysis') {
            steps {
                sh '''
                    pip install bandit
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                archiveArtifacts artifacts: 'bandit-output.xml'
            }
        }
    }
}
