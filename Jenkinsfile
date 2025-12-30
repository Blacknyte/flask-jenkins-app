pipeline {
    agent any

    environment {
        VENV = "venv"
        DEPLOY_DIR = "C:\\flask_deploy"
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
                bat '''
                python -m venv %VENV%
                call %VENV%\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests with pytest...'
                bat '''
                call %VENV%\\Scripts\\activate
                python -m pytest
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo 'Preparing application for deployment...'
                bat '''
                if not exist build mkdir build
                copy app.py build\\
                copy requirements.txt build\\
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Simulating deployment...'
                bat '''
                if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%
                xcopy /Y /E build\\* %DEPLOY_DIR%\\
                '''
            }
        }
    }

    post {
        success { echo 'Pipeline executed successfully!' }
        failure { echo 'Pipeline failed. Check logs!' }
    }
}

