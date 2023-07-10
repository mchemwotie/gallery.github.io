pipeline {
    agent any
    
    tools {nodejs "node"}

    stages {
        stage('Start') {
            steps {
                echo 'started running the pipeline'
            }
        }
        stage('Clone the repository') {
            steps {
                git url: 'https://github.com/AlbertNjaneKimani/gallery.git', branch: 'master'
            }
        }
        stage('Get Latest Commit') {
            steps {
                sh '''
                   export COMMIT=$(git log --oneline | awk '{print $1}' | head -n 1)
                   echo $COMMIT
                   '''
            }
        }
        stage('Install node dependencies') {
            steps {
                sh '''
                   npm install
                   '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                npm test
                '''
            }
        }
        stage('Deploy to Render') {
            steps {
                sh '''
                   curl -X POST https://api.render.com/deploy/srv-cgbni61mbg55nqndm9n0?key=$COMMIT
                   '''
            }
        }
         stage('Send Slack Notification') {
            when {
                expression { true}
            }
            steps {
                slackSend channel: '#mercyip', color: 'good', message: "Deployment successful for build ID: ${env.BUILD_ID}. Site available at https://jenkins3.onrender.com"
            }
        }
        stage('End') {
            steps {
                echo 'The build has ended'
            }
        }
    }
    post {
