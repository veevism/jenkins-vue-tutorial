pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-access-token') 
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/veevism/jenkins-vue-tutorial.git' , credentialsId: 'github-access-token'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {

                    bat 'npm install'
                }
            }
        }

        stage('Build Project') {
            steps {
                bat 'npm run build'
            }
        }

        // stage('Test Project') {
        //     steps {
        //         bat 'npm run test'
        //     }
        // }



        stage('Deploy Project') {
            steps {
                echo 'Deploying the project...'

                script {
                    bat '''
                    rmdir /S /Q "D:/deployment" || true
                    mkdir "D:/deployment"
                    xcopy /S /E /Y "dist" "D:/deployment"

                    cd /D "D:/deployment"
                    npm install -g pm2
                    pm2 stop vue-app || true  
                    pm2 start npm --name "vue-app" -- run serve -- --port 3000  
                    pm2 save 
                    pm2 list  
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
