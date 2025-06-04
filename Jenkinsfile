pipeline {
    agent any
    parameters {
        choice(name: 'ACTION', choices: ['deploy', 'destroy'], description: 'Choose deploy or destroy')
    }
    environment {
        KUBECONFIG = "~/.kube/config"
    }
    stages {
        stage('Clone Repo') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/omerH299/simple-web.git'
            }
        }
        stage('Set Context') {
            steps {
                sh 'az login -i'
                sh 'az aks get-credentials -n devops-interview-aks -g devops-interview-rg'
                sh 'kubelogin convert-kubeconfig -l msi'
            }
        }
        stage('Deploy or Destroy') {
            steps {
                script {
                    if (params.ACTION == 'deploy') {
                        sh 'helm upgrade --install simple-web . -n omerhi'
                    } else {
                        sh 'helm uninstall simple-web -n omerhi'
                    }
                }
            }
        }
    }
}
