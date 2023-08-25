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
                        backendProcess = startBackend() // Start backend and capture process reference
                    }
                    
                    dir('frontend') {
                        startFrontend() // Start frontend
                    }

                    sleep 10 // Wait for services to start
                    checkBackendAlive(backendProcess) // Check if backend is alive
                }
            }
        }
    }
}

def startBackend() {
    return bat(script: 'venv\\Scripts\\python app.py', returnStatus: true) // Adjust the path accordingly for Windows
}

def startFrontend() {
    bat(script: 'npm start', returnStatus: true)
}

def checkBackendAlive(exitCode) {
    if (exitCode != 0) {
        error('Backend process failed to start.')
    }
}
