pipeline {
    agent any

    environment {
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '<EC2-PUBLIC-IP>'
        DEPLOY_KEY  = credentials('your-ec2-key') // Add this as a Jenkins SSH credential
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
                sshagent (credentials: ['your-ec2-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST 'mkdir -p ~/app'
                        scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:~/app/
                    """
                }
            }
        }
    }
}
