pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                sh '''
                python3 -m venv venv
                source venv/bin/activate && pip install -r requirements.txt
                '''
            }
        }

        stage('Static Analysis - pylint') {
            steps {
                sh '''
                source venv/bin/activate && pylint my-app > pylint-report.txt || true
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                source venv/bin/activate && pip install pytest
                source venv/bin/activate && pytest test_my_app.py --junitxml=pytest-report.xml || true
                '''
            }
            post {
                always {
                    junit 'pytest-report.xml'
                }
            }
        }
    }

    artifacts {
        always {
            archiveArtifacts artifacts: 'pylint-report.txt', onlyIfSuccessful: true
        }
    }
}
