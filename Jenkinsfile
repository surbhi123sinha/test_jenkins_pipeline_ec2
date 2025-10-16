pipeline {
    agent any

    environment {
        ENDPOINT = 'https://11fb1wqzn4.execute-api.us-east-1.amazonaws.com'
        MY_IP = '165.225.120.94/32'
        ACTION = "${params.ACTION}"
        INSTANCE_ID = "${params.INSTANCE_ID}"
    }

    parameters {
        string(name: 'ACTION', defaultValue: 'create', description: 'EC2 action: create, start, stop, terminate')
        string(name: 'INSTANCE_ID', defaultValue: '', description: 'EC2 instance ID (if needed)')
    }

    stages {
        stage('Debug Info') {
            steps {
                echo "Pipeline started with ACTION=${params.ACTION}, INSTANCE_ID=${params.INSTANCE_ID}"
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing Python dependencies..."
                bat '''
                    python --version
                    python -m pip install --upgrade pip
                    pip install requests
                '''
            }
        }

        stage('Run Lambda API Trigger') {
            steps {
                echo "Running Lambda API Script..."
                bat '''
                    python lambda_api_runner.py
                '''
            }
        }
    }
}
