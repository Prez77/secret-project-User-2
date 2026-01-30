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
            steps {
                // Applies the deployment logic to your K3s cluster
                sh 'kubectl apply -f - <<EOF\n' +
                   'apiVersion: apps/v1\n' +
                   'kind: Deployment\n' +
                   'metadata:\n' +
                   '  name: web-app\n' +
                   'spec:\n' +
                   '  replicas: 1\n' +
                   '  selector:\n' +
                   '    matchLabels:\n' +
                   '      app: web-app\n' +
                   '  template:\n' +
                   '    metadata:\n' +
                   '      labels:\n' +
                   '        app: web-app\n' +
                   '    spec:\n' +
                   '      containers:\n' +
                   '      - name: nginx\n' +
                   '        image: local-web-app:latest\n' +
                   '        imagePullPolicy: Never\n' +
                   '---\n' +
                   'apiVersion: v1\n' +
                   'kind: Service\n' +
                   'metadata:\n' +
                   '  name: web-service\n' +
                   'spec:\n' +
                   '  type: NodePort\n' +
                   '  selector:\n' +
                   '    app: web-app\n' +
                   '  ports:\n' +
                   '    - port: 80\n' +
                   '      targetPort: 80\n' +
                   '      nodePort: 30080\n' +
                   'EOF'
            }
        }
    }
}
