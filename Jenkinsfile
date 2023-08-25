pipeline {
    agent any

    stages {
        stage('Build Backend') {
            steps {
                // ... Build backend as before ...
            }
        }
        
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                dir('backend') {
                    sh '. venv/bin/activate && python app.py &'
                }
                dir('frontend/build') {
                    sh 'npm install -g serve'
                    sh 'serve -s . -p 3000 &'
                }
                // Add a delay to allow both servers to start before proceeding
                sleep time: 30, unit: 'SECONDS'
            }
        }
    }
}
