pipeline {
    agent { label 'dev-eko' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/ekocapucho/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                -Dsonar.projectKey=belajar \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.8.124:9000 \
                -Dsonar.login=sqp_8667dc84b02cc19ce4b3e4e744b22a4e5a837332
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}