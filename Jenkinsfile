pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/prabhatraghav/star-agile-banking-finance/', branch: "master"
            }
        }
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t finbankimg .'
                sh 'docker tag finbankimg:latest techomaniac83/finbankimgaddbook:latest'
            }
        }

        stage('Docker login and push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
                    sh 'docker push techomaniac83/finbankimgaddbook:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                    sh 'docker run -itd --name finbank -p 9090:8090 techomaniac83/finbankimgaddbook:latest'
                }
            }
    }
        post {
            success {
                echo 'Pipeline successfully executed!'
            }
            failure {
                echo 'Pipeline failed!'
            }
        }
}
