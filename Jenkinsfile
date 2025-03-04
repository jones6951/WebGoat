pipeline {
    agent any
    environment {
        DETECT_PROJECT_NAME = "MJ-WebGoatTest1"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B package'
            }
        }
        stage('Black Duck SCA') {
            steps {
                security_scan product: 'blackducksca',
                    blackducksca_scan_failure_severities: 'BLOCKER',
                    include_diagnostics: false
            }
        }
    }
}
