pipeline {
    agent any

    environment {
        VENV = "venv"
        DEPLOY_DIR = "/tmp/flask_deploy"
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning GitHub repository...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests with pytest...'
                sh '''
                . $VENV/bin/activate
                pytest
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo 'Preparing application for deployment...'
                sh '''
                mkdir -p build
                cp -r app.py requirements.txt build/
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Simulating deployment...'
                sh '''
                mkdir -p $DEPLOY_DIR
                cp -r build/* $DEPLOY_DIR/
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
