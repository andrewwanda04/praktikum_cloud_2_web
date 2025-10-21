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
                echo Matikan dan hapus container lama...
                docker stop laravel_app || echo "laravel_app tidak berjalan"
                docker rm laravel_app || echo "laravel_app sudah dihapus"
                docker stop nginx_server || echo "nginx_server tidak berjalan"
                docker rm nginx_server || echo "nginx_server sudah dihapus"
                docker stop mysql_db || echo "mysql_db tidak berjalan"
                docker rm mysql_db || echo "mysql_db sudah dihapus"

                echo Jalankan ulang docker-compose...
                docker-compose down || exit 0
                docker-compose up -d
                '''
            }
        }

        stage('Verify Container Running') {
            steps {
                bat '''
                timeout /t 10
                echo Cek koneksi ke Laravel...
                curl -I http://localhost:8081 || echo "Gagal akses Laravel di port 8081"
                
                echo.
                echo ==== ISI HALAMAN ====
                curl http://localhost:8081 || echo "Gagal ambil isi halaman"
                echo =====================
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
