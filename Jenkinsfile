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
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                    echo Installing Python dependencies...
                    python -m pip install --upgrade pip
                    pip install requests
                '''
            }
        }

        stage('Run Lambda API Trigger') {
            steps {
                bat '''
                    echo Running Lambda API Script...
                    python lambda_api_runner.py || exit /b 1
                '''
            }
        }
    }
}
