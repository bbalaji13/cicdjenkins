pipeline {
    agent any
    
    stages {
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install -r requirements.txt'
                }
            }
        }
        
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build' // Build the frontend
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    def backendProcess // To store the backend process

                    dir('backend') {
                        sh 'venv/bin/python app.py &' // Start backend in the background using virtual environment
                        backendProcess = lastBackgroundProcess() // Store the background process reference
                    }
                    
                    dir('frontend') {
                        sh 'npm start' // Start frontend in the background
                    }

                    sleep 10 // Wait for services to start
                    checkBackendAlive(backendProcess) // Check if backend is alive
                }
            }
        }
    }
}

def lastBackgroundProcess() {
    return sh(script: 'echo $!', returnStdout: true).trim()
}

def checkBackendAlive(processId) {
    sh "ps -p $processId"
}
