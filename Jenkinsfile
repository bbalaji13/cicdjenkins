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
    return build(job: 'backend-start', wait: false) // Start backend job in the background
}

def startFrontend() {
    return build(job: 'frontend-start', wait: false) // Start frontend job in the background
}

def checkBackendAlive(build) {
    echo "Waiting for backend to start..."
    build.log.eachLine { line ->
        if (line.contains("Backend started")) {
            echo "Backend started successfully."
            return
        }
    }
    error('Backend process failed to start.')
}
