pipeline {
    agent any;

    stages {

        stage('Code Clone') {
            steps {
                git url: "https://github.com/harshitgarg-03/two-tier-flask-app.git", branch: "master"
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh 'echo -n "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin'
                    sh 'docker tag two-tier-flask-app $dockerHubUser/two-tier-flask-app:latest'
                    sh 'docker push $dockerHubUser/two-tier-flask-app:latest'
                }
            }
        }
        
        
        stage('Deploy') {
            steps {
                sh "docker compose pull flask-app"
                sh "docker compose up -d"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
