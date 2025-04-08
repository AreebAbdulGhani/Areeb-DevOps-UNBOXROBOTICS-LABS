pipeline {
    agent any
    stages {
        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh """
                    docker run --rm -v \$HOME/.kube:/root/.kube \
                      argoproj/argocd:latest argocd app create devops-log-pipeline \
                      --repo https://github.com/AreebAbdulGhani/Areeb-DevOps-UNBOXROBOTICS-LABS.git \
                      --path k8s \
                      --dest-server https://kubernetes.default.svc \
                      --dest-namespace default || true

                    docker run --rm -v \$HOME/.kube:/root/.kube \
                      argoproj/argocd:latest argocd app sync devops-log-pipeline
                    """
                }
            }
        }
    }
}
