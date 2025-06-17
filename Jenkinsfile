pipeline {
    agent any

    environment {
        DEPLOY_USER = 'azureuser'  // Replace if your VM username is different
        DEPLOY_HOST = '40.81.23.80' // Replace with your Azure VM Public IP
        AZURE_CREDENTIALS = credentials('jenkins-sp-nimra') // Your Azure SP ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NIMRA46/devopsproject.git'
            }
        }

        stage('Azure Login') {
            steps {
                sh '''
                    az login --service-principal -u $AZURE_CREDENTIALS_USR -p $AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN
                    az account set --subscription 08039bca-3858-4c11-97a7-e199f8325273
                '''
            }
        }

        stage('Deploy to Azure VM') {
            steps {
                sshagent (credentials: ['azure-vm-ssh']) {  // Replace with your SSH Key ID
                    sh '''
                        ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST 'mkdir -p ~/app'
                        scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:~/app/
                    '''
                }
            }
        }
    }
}
