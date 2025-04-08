pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    def services = ['loki', 'promtail', 'grafana']
                    services.each { service ->
                        sh """
                        docker build -t areeb01/\${service}:latest -f \${service}/Dockerfile \${service}
                        docker login -u \${DOCKER_HUB_CREDENTIALS_USR} -p \${DOCKER_HUB_CREDENTIALS_PSW}
                        docker push areeb01/\${service}:latest
                        """
                    }
                }
            }
        }
        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh """
                    argocd app create devops-log-pipeline --repo https://github.com/AreebAbdulGhani/Areeb-DevOps-UNBOXROBOTICS-LABS.git --path k8s --dest-server https://kubernetes.default.svc --dest-namespace default
                    argocd app sync devops-log-pipeline
                    """
                }
            }
        }
    }
}
