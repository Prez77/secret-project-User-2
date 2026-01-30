pipeline {
    agent any

    environment {
        // This ensures kubectl knows where the credentials are
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'subway', url: 'https://github.com/prez77/secret-project-User-2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t local-web-app:latest .'
            }
        }

        stage('Deploy to K3s') {
            steps {
                sh '''
                kubectl apply -f - <<EOF
                apiVersion: apps/v1
                kind: Deployment
                metadata:
                  name: web-app
                spec:
                  replicas: 1
                  selector:
                    matchLabels:
                      app: web-app
                  template:
                    metadata:
                      labels:
                        app: web-app
                    spec:
                      containers:
                      - name: nginx
                        image: local-web-app:latest
                        imagePullPolicy: Never
                ---
                apiVersion: v1
                kind: Service
                metadata:
                  name: web-service
                spec:
                  type: NodePort
                  selector:
                    app: web-app
                  ports:
                    - port: 80
                      targetPort: 80
                      nodePort: 30080
                EOF
                '''
            }
        }
    }
}
