pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/athabrani/sast-demo-app.git', branch: 'main'
            }
        }

        stage('Setup Virtualenv') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    . $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install bandit
                '''
            }
        }

        stage('SAST Analysis') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    bandit -r . -f xml -o bandit-output.xml || echo "Bandit finished with issues"
                    bandit -r . -f txt || echo "Bandit text output"
                '''
            }
        }

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'bandit-output.xml', fingerprint: true
            }
        }
    }
}
