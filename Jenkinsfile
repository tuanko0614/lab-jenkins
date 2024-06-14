pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/tuanko0614/lab-jenkins.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'docker build -t tuandt0614/local-jenkins:latest .'
                }
            }
        }

        stage('Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'usr')]) {
                    script {
                        // Tạo file tạm để lưu mật khẩu
                        writeFile file: 'docker_password.txt', text: "${password}"
                        // Đăng nhập vào Docker Hub
                        bat 'docker login -u %usr% --password-stdin < docker_password.txt'
                        // Xóa file tạm sau khi đăng nhập
                        bat 'del docker_password.txt'
                    }
                }
            }
        }

        stage('Push') {
            steps {
                bat 'docker push tuandt0614/local-jenkins:latest'
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}
