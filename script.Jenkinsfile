pipeline {
    agent any

    environment {
        REPO_URL = ''  // repo url
        COMPOSE_PROJECT_NAME = ''  // project name
        DEPLOY_PATH = 'folder/path/'  // Change to your actual deployment path
        SERVER_USER = ''         // Change to your server's SSH user
        SERVER_IP = ''    // Change to your server's IP address
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

 stage('Deploy to Server') {
    steps {
        sshagent(credentials: ['ssh-credentials']) {  // Use your credentials ID
            sh '''
                echo "Copying files to server..."
                scp -r * ${SERVER_USER}@${SERVER_IP}:${DEPLOY_PATH}

                echo "Deploying on server..."
                ssh ${SERVER_USER}@${SERVER_IP} << 'EOF'
cd ${DEPLOY_PATH}
echo "Stopping existing services..."
docker-compose down
echo "Building and starting services..."
docker-compose up -d --build
EOF
            '''
        }
    }
}

    }

    post {
        always {
            cleanWs()
        }
    }
}
