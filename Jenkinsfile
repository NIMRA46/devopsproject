pipeline {
    agent any

    environment {
        DEPLOY_USER = 'azureuser'
        DEPLOY_HOST = '40.81.23.80'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NIMRA46/devopsproject.git'
            }
        }

        stage('Azure Login') {
            steps {
                withCredentials([azureServicePrincipal(
                    credentialsId: 'jenkins-sp-nimra',
                    subscriptionIdVariable: 'SUBSCRIPTION_ID',
                    clientIdVariable: 'CLIENT_ID',
                    clientSecretVariable: 'CLIENT_SECRET',
                    tenantIdVariable: 'TENANT_ID'
                )]) {
                    sh '''
                        az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET --tenant $TENANT_ID
                        az account set --subscription $SUBSCRIPTION_ID
                    '''
                }
            }
        }

        stage('Deploy to Azure VM') {
            steps {
                sshagent (credentials: ['azure-vm-ssh']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST 'mkdir -p ~/app'
                        scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:~/app/
                    '''
                }
            }
        }
    }
}
