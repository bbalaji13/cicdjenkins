pipeline {
    agent any
    
    stages {
        stage('Build Backend') {
            steps {
                dir('backend') {
                    script {
                        sh 'python3 -m venv venv'
                        sh '. venv/bin/activate && pip install -r requirements.txt'
                    }
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
        stage('Deploy Backend') { // Renamed stage name
            steps {
                dir('backend') {
                    script {
                        sh 'python3 -m venv venv'
                        sh '. venv/bin/activate && python app.py'
                    }
                }
            }
        }
        stage('Deploy Frontend') { // Renamed stage name
            steps {
                dir('frontend') {
                    script {
                        sh 'npm run build'
                        sh 'npm install -g serve'
                        sh 'serve -s build -p 3000'
                    }
                }
            }
        }
    }
}
