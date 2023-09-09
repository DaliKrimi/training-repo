pipeline {
    agent any

    environment {
        commit_id = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
    }

    stages {
        stage('Preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit_id"
            }
        }
       
    }
}
