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
                sh 'bandit -f xml -o bandit-output.xml -r .'
                echo 'Bandit scan completed'
            }
        }

        stage('Process Results') {
            steps {
                recordIssues(
                    tools: [bandit(pattern: 'bandit-output.xml')],
                    qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]]
                )
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'bandit-output.xml', allowEmptyArchive: true
        }
    }
}
