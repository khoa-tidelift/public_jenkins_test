pipeline {
    agent any
    environment {
// Always store API keys in Jenkins credential store
// ORG key should be either an Organization API key or Project API key
        TIDELIFT_PROJECT_API_KEY = credentials('tidelift-project-key')
// These are needed to run alignments and also to create the project. Ideally these are read from
// configuration in Jenkins or from local metadata stored in the repository being checked out. Test.
        TIDELIFT_PROJECT_NAME = 'Test123'
        TIDELIFT_ORGANIZATION = 'team/khoa-tidelift'
        TIDELIFT_CATALOG = 'khoa-cat'
    }
    stages {
        stage('Howdy') {
            steps {
                echo 'Howdy'
            }
        }
// Download the Tidelift CLI and make it executable
        stage('Downloading Tidelift CLI') {
            steps {
                  sh 'curl -s -o ./tidelift https://download.tidelift.com/cli/tidelift_darwin'
                  sh 'chmod +x ./tidelift'
            }
        }
        stage('Running Tidelift Alignment') {
            steps {
              withEnv(['TIDELIFT_API_KEY=${TIDELIFT_PROJECT_API_KEY}']){
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                      sh "./tidelift alignment save --wait --project ${TIDELIFT_PROJECT_NAME} --organization ${TIDELIFT_ORGANIZATION}"
                 }
              }
            }
        }
    }
}
