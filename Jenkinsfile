pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/betawins/Trading-UI.git'
            }
        }

        stage('Verify Node & NPM') {
            steps {
                sh '''
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Audit (Optional)') {
            steps {
                sh 'npm audit fix || true'
            }
        }

        stage('Build UI') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Start App with PM2') {
            steps {
                sh '''
                pm2 delete Trading-UI || true
                pm2 --name Trading-UI start npm -- start
                pm2 save
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Trading-UI deployed successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
        always {
            cleanWs()
        }
    }
}
