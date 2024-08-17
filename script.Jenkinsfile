pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/GhaithRouahi/React-X-Express-crud-app.git'
        COMPOSE_PROJECT_NAME = 'basic-app'
        DEPLOY_PATH = '/home/ubuntu/'  // Change to your actual deployment path
        SERVER_USER = 'ubuntu'         // Change to your server's SSH user
        SERVER_IP = '54.211.21.101'    // Change to your server's IP address
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
