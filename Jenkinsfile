pipeline {
    agent any
    
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World!'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'echo "This is the build stage"'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'echo "Running tests"'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sh 'echo "Application deployed successfully"'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
