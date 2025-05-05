pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/maruwrks/sast-demo-app.git'
            }
        }

        stage('SAST Analysis') {
            steps {
                // Continue even if Bandit finds issues (exit code 1)
                sh 'bandit -f xml -o bandit-output.xml -r . || true'
                echo 'Bandit scan completed'
            }
        }

        stage('Process Results') {
            steps {
                recordIssues(
                    tools: [issues(pattern: 'bandit-output.xml', name: 'Bandit', type: 'bandit')],
                    qualityGates: [
                        [threshold: 1, type: 'TOTAL_HIGH', unstable: true],
                        [threshold: 1, type: 'TOTAL_ERROR', unstable: true]
                    ]
                )
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'bandit-output.xml', allowEmptyArchive: true
        }
        unstable {
            echo 'Build marked as unstable due to security findings'
        }
        failure {
            echo 'Build failed for reasons other than security findings'
        }
    }
}
