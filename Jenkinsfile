pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-app"
        CONTAINER_APP = "laravel_app"
        CONTAINER_WEB = "nginx_server"
        CONTAINER_DB  = "mysql_db"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Mengambil source code dari GitHub...'
                git branch: 'main', url: 'https://github.com/andrewwanda04/praktikum_cloud_2_web.git'
            }
        }

        stage('Build and Deploy with Docker Compose') {
            steps {
                echo '🚀 Membangun image dan menjalankan container...'
                sh '''
                docker compose down || true
                docker compose up -d --build
                '''
            }
        }

        stage('Check Running Containers') {
            steps {
                echo '🔍 Mengecek container yang berjalan...'
                sh 'docker ps'
            }
        }

        stage('Health Check') {
            steps {
                echo '🩺 Mengecek apakah Laravel sudah aktif di port 8081...'
                sh 'sleep 10 && curl -I http://localhost:8081 || true'
            }
        }
    }

    post {
        success {
            echo '✅ Laravel berhasil dijalankan via Jenkins (port 8081)'
        }
        failure {
            echo '❌ Build gagal, cek console log di Jenkins untuk detail error.'
        }
    }
}
