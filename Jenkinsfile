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
                    sh 'npm run build'
                }
            }
        }
        
        stage('Deploy to Nginx') {
            steps {
                script {
                    // Copy frontend build files to Nginx's HTML root
                    sh 'sudo cp -r frontend/build/* /usr/share/nginx/html/'
                    
                    // Restart Nginx to apply changes
                    sh 'sudo service nginx restart'
                }
            }
        }
        
        stage('Deploy Backend') {
            steps {
                dir('backend') {
                    // Start the backend application using the virtual environment
                    sh 'pip install flask'
                    sh 'python3 app.py'
                }
            }
        }
    }
}
