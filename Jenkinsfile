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
                    # Install bandit secara global
                    pip3 install --user bandit || python3 -m pip install --user bandit
                '''
            }
        }
        
        stage('SAST Analysis') {
            steps {
                sh '''
                    # Jalankan bandit langsung
                    bandit -f xml -o bandit-output.xml -r . || true
                    
                    # Alternatif jika bandit command tidak ditemukan
                    python3 -m bandit -f xml -o bandit-output.xml -r . || true
                '''
                
                // Untuk memproses hasil scan (butuh Warnings NG plugin)
                recordIssues(
                    tools: [banditParser(pattern: 'bandit-output.xml')],
                    qualityGates: [[threshold: 1, type: 'TOTAL_HIGH', unstable: true]]
                )
                
                // Alternatif sederhana jika hanya ingin menyimpan hasil
                archiveArtifacts artifacts: 'bandit-output.xml', allowEmptyArchive: true
            }
        }
    }
}
