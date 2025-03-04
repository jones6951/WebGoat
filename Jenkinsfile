pipeline {
    agent any
    environment {
        DETECT_PROJECT_NAME = "MJ-WebGoatTest1"
        REPO_NAME = "MJ-WebGoatTest1"
        BRANCH_NAME = "main"
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
                    include_diagnostics: false
            }
        }
        stage('Black Duck SAST') {
            steps {
                security_scan product: 'srm',
                    srm_assessment_types: 'SAST',
                    srm_project_name: "$REPO_NAME",
                    srm_branch_name: "$BRANCH_NAME"
            }
        }
            
            
            steps {
                security_scan product: 'blackducksca',
                    include_diagnostics: false
            }
        }
    }
}
