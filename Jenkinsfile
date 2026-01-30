pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pulls the subway branch automatically
                git branch: 'subway', url: 'https://github.com/prez77/secret-project-User-2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Uses the current workspace where Jenkins just pulled the code
                sh 'docker build -t local-web-app:latest .'
            }
        }

      stage('Deploy to K3s') {
            environment {
                // This tells Jenkins to use its local copy of the config
                KUBECONFIG = '/var/lib/jenkins/.kube/config'
            }
            steps {
                sh 'kubectl apply -f - <<EOF\n' +
                // ... (the rest of your deployment code)
            }
        }
    }
}
