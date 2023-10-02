pipeline{
    agent any
     environment {
       BUILD_NUMBER="${BUILD_NUMBER}"
    }
    stages{
        stage('Git Clone')
        {
            steps{
            git branch: 'main', credentialsId: 'afdd31f6-eed3-41b1-99fa-255552ce90d9', url: 'https://github.com/manubolutharunsaireddyms/java-web-app-docker.git'
            }
        }
        stage('Maven Clean Package')
        {
            steps{
                 sh 'mvn clean package'
            }
        }
        stage('Build Docker Image')
        {
            steps{
                sh "docker build -t awstharun/java-web-app:${BUILD_NUMBER} ."
                echo "${BUILD_NUMBER}"
            }
        }
        stage('Docker push')
        {
            steps{
                sh "docker login -u awstharun -p Ur16cs035#"
                sh "docker push docker.io/awstharun/java-web-app:${BUILD_NUMBER}"
            }
        }
        stage('Deploy Application')
        {
            steps{
                sshagent(['Docker_SSH']) {
                     sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.213 sudo docker pull awstharun/java-web-app:${BUILD_NUMBER}"
                     sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.213 sudo docker rm -f javawebappcontainer || true'
                     sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.213 sudo docker run -d -p 8080:8080 --name javawebappcontainer awstharun/java-web-app:${BUILD_NUMBER}"
                }
            }
        }
    }
}