pipeline {
    agent any

    environment {
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = 'EC2_PUBLIC_IP_HERE'   // <-- Replace with your actual EC2 public IP
        DEPLOY_KEY  = credentials('ec2-ssh-key') // <-- Use the exact ID you gave in credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/NIMRA46/devopsproject.git'
            }
        }

        stage('Build') {
            steps {
                echo "Build step (if needed)"
            }
        }

        stage('Test') {
            steps {
                echo "Run tests"
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST 'mkdir -p ~/app'
                        scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:~/app/
                    """
                }
            }
        }
    }
}

