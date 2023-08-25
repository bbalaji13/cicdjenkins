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
                }
            }
        }
        // stage('Deploy') {
        //     steps {
        //         dir('frontend') {
        //             sh 'npm run build'
        //         }
        //         dir('backend') {
        //             sh 'source venv/bin/activate && python app.py &'
        //         }
        //     }
        // }
        stage('Deploy') {
            steps {
                dir('frontend') {
                    sh 'npm start &'
                }
                dir('backend') {
                    sh 'source venv/bin/activate && python app.py &'
                }
                sleep 10 // Wait for services to start
                    checkBackendAlive(backendProcess) // Check if backend is alive
            }
        }
    }
}
