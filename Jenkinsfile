pipeline {
    agent any

    environment {
        ARGOCD_SERVER = "https://host.docker.internal:8081"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh """
                    docker run --rm \
                      -v \$HOME/.kube:/root/.kube \
                      argoproj/argocd:latest \
                      argocd app create devops-log-pipeline \
                        --repo https://github.com/AreebAbdulGhani/Areeb-DevOps-UNBOXROBOTICS-LABS.git \
                        --path k8s \
                        --dest-server https://kubernetes.default.svc \
                        --dest-namespace default \
                        --server \$ARGOCD_SERVER || true
                    """

                    sh """
                    docker run --rm \
                      -v \$HOME/.kube:/root/.kube \
                      argoproj/argocd:latest \
                      argocd app sync devops-log-pipeline \
                        --server \$ARGOCD_SERVER
                    """
                }
            }
        }
    }
}
