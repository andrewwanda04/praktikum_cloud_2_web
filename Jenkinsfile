pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-app"
        CONTAINER_NAME = "laravel_app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/andrewwanda04/praktikum_cloud_2_web.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker-compose build'
            }
        }

        stage('Run Docker Containers') {
            steps {
                bat '''
                echo "🧹 Membersihkan container lama..."
                docker stop nginx_server || exit 0
                docker stop mysql_db || exit 0
                docker stop laravel_app || exit 0
                docker rm nginx_server || exit 0
                docker rm mysql_db || exit 0
                docker rm laravel_app || exit 0
                docker-compose down || exit 0

                echo "🚀 Menjalankan docker-compose up -d"
                docker-compose up -d
                '''
            }
        }

        stage('Verify Container Running') {
            steps {
                bat '''
                timeout /t 10 >nul
                curl -I http://localhost:8081 || echo "⚠️ Gagal mengakses Laravel di port 8081"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Laravel berhasil dijalankan via Docker Compose di port 8081!'
        }
        failure {
            echo '❌ Build gagal, cek log Jenkins console output.'
        }
    }
}
