pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                input(
                    id: 'manual-approval',
                    message: 'Lanjutkan ke tahap Deploy?',
                    ok: 'Proceed',
                    submitter: 'user',
                    parameters: [
                        booleanParam(defaultValue: false, description: 'Klik Proceed untuk melanjutkan', name: 'proceed')
                    ]
                )
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 

                // Menjeda eksekusi selama 1 menit
                sh 'sleep 60'
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}