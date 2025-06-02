pipeline {
    agent any

    environment {
        IMAGE_NAME = 'php-web-app'
        IMAGE_TAG = '1'
        KUBECONFIG_CRED_ID = 'ssh-deploy-key'
        DEPLOY_USER = 'nama_user_ssh'
        DEPLOY_HOST = 'ip_server_anda'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/your-repo/web-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sshagent (credentials: [env.KUBECONFIG_CRED_ID]) {
                    sh """
                    scp deployment.yaml service.yaml nginx-config.yaml ${DEPLOY_USER}@${DEPLOY_HOST}:/home/${DEPLOY_USER}/
                    ssh ${DEPLOY_USER}@${DEPLOY_HOST} << EOF
                        kubectl apply -f nginx-config.yaml
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    EOF
                    """
                }
            }
        }
    }
}
