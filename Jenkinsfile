pipeline {
    agent any
    environment {
        SRM_TOKEN = credentials('SRM_TOKEN')
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
        stage('Fail the build') {
            steps {
                script {
                    sh '''
                        curl -s -L https://raw.githubusercontent.com/jones6951/srm-scripts/main/runAnalysis.sh > /tmp/runAnalysis.sh
                        chmod +x /tmp/runAnalysis.sh
                        breakBuild=$(/tmp/runAnalysis.sh --url=${SRM_URL} --apikey={SRM_TOKEN} --project=$REPO_NAME --branch=$BRANCH_NAME)
                    '''
                    if (breakBuild == 'true') {
                        currentBuild.result = 'FAILURE'
                        error('Failing the build')
                    }
                }
            }
        }
    }
}
