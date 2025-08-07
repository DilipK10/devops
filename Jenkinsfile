pipeline {
    agent any
    
    environment {
        // Define environment variables (optional)
        PYTHON_VENV = "python-venv"
    }
    
    stages {
        // Clone the repository from GitHub
        stage('Clone') {
            steps {
                script {
                    echo 'Cloning repository from GitHub...'
                    // Clone the repo using Git
                    checkout scm
                }
            }
        }
        
        // Set up Python environment and install dependencies
        stage('Build') {
            steps {
                script {
                    echo 'Setting up Python environment...'
                    
                    // Install python3-venv if not already installed
                    sh '''
                    dpkg -l | grep -q python3-venv || sudo apt-get update && sudo apt-get install -y python3.10-venv
                    '''
                    
                    // Create a virtual environment
                    sh '''
                    python3 -m venv ${PYTHON_VENV}
                    source ${PYTHON_VENV}/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }
        
        // Run tests using pytest
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // Run pytest to execute tests
                    sh '''
                    source ${PYTHON_VENV}/bin/activate
                    pytest --maxfail=1 --disable-warnings -q
                    '''
                }
            }
        }
        
        // Deploy the application (you can customize this to your needs)
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    // Add deployment commands here (e.g., AWS, GCP, Kubernetes, etc.)
                    // For example, using Docker, Kubernetes, or cloud-specific commands:
                    // sh 'docker build -t my-app .'
                    // sh 'docker push my-app'
                    // sh 'kubectl apply -f deploy.yaml'
                    
                    // In this example, we just echo a deployment message
                    echo "Deployment commands go here"
                }
            }
        }
    }
    
    post {
        always {
            // Clean up environment after the pipeline run
            echo 'Cleaning up...'
            sh "rm -rf ${PYTHON_VENV}"
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
