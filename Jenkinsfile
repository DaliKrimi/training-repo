def commit_id
pipeline {
    agent any

    stages{
        stage('Preparation'){
            steps{
                checkout scm
                bat "git rev-parse --short HEAD > .git/commit_id"
                script{
                     commit_id = readFile('.git/commit_id').trim()
                }
            }
        }
        // stage('Image Build') {
        //     steps{
        //         echo 'Building'
        //         sh "scp -r -i \$(minikube ssh-key) ./* docker@\${minikube ip}:~"
        //         sh "minikube ssh 'docker build -t webapp:${commit_id} ./'"
        //         echo "build complete"
        //     }
        // }
        stage('Deploy'){
            steps{
                echo "Deploying for kubernetes"
                bat "sed -i -r 's|richardchesterwood/k8s-fleetman-webapp-angular:release2|webapp:1|' ./manifests/workloads.yaml"
                bat 'kubectl get all'
                bat 'kubectl apply -f ./manifests/'
                bat 'kubectl get all'
                echo 'deployement complete'
            }
        }
    }
}