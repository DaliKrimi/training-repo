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
        stage('Image Build') {
            steps {
                echo 'Building'
                sh "scp -r -i \$(minikube ssh-key) ./* docker@\${minikube ip}:~"
                sh "minikube ssh 'docker build -t webapp:${commit_id} ./'"
                echo "Build complete"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying for Kubernetes"
                sh "sed -i -r 's|richardchesterwood/k8s-fleetman-webapp-angular:release2|webapp:${commit_id}|' ./manifests/workloads.yaml"
                sh 'kubectl get all'
                sh 'kubectl apply -f ./manifests/'
                sh 'kubectl get all'
                echo 'Deployment complete'
            }
        }
    }
}
