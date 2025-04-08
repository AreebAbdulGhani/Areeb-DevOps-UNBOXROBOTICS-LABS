pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/AreebAbdulGhani/Areeb-DevOps-UNBOXROBOTICS-LABS.git'
            }
        }

        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh '''
                        docker run --rm -v $HOME/.kube:/root/.kube argoproj/argocd:latest argocd app create devops-log-pipeline \
                            --repo https://github.com/AreebAbdulGhani/Areeb-DevOps-UNBOXROBOTICS-LABS.git \
                            --path k8s \
                            --dest-server https://kubernetes.default.svc \
                            --dest-namespace default \
                            --server host.docker.internal:8081 || true
                    '''

                    sh '''
                        docker run --rm -v $HOME/.kube:/root/.kube argoproj/argocd:latest argocd app sync devops-log-pipeline \
                            --server host.docker.internal:8081
                    '''
                }
            }
        }
    }
}
