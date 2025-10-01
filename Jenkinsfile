// @Library("Shared") _
// pipeline{
    
//     agent { label "dev"};
    
//     stages{
//         stage("Code Clone"){
//             steps{
//                script{
//                    clone("https://github.com/LondheShubham153/two-tier-flask-app.git", "master")
//                }
//             }
//         }
//         stage("Trivy File System Scan"){
//             steps{
//                 script{
//                     trivy_fs()
//                 }
//             }
//         }
//         stage("Build"){
//             steps{
//                 sh "docker build -t two-tier-flask-app ."
//             }
            
//         }
//         stage("Test"){
//             steps{
//                 echo "Developer / Tester tests likh ke dega..."
//             }
            
//         }
//         stage("Push to Docker Hub"){
//             steps{
//                 script{
//                     docker_push("dockerHubCreds","two-tier-flask-app")
//                 }  
//             }
//         }
//         stage("Deploy"){
//             steps{
//                 sh "docker compose up -d --build flask-app"
//             }
//         }
//     }

// post{
//         success{
//             script{
//                 emailext from: 'mentor@trainwithshubham.com',
//                 to: 'mentor@trainwithshubham.com',
//                 body: 'Build success for Demo CICD App',
//                 subject: 'Build success for Demo CICD App'
//             }
//         }
//         failure{
//             script{
//                 emailext from: 'mentor@trainwithshubham.com',
//                 to: 'mentor@trainwithshubham.com',
//                 body: 'Build Failed for Demo CICD App',
//                 subject: 'Build Failed for Demo CICD App'
//             }
//         }
//     }
// }

pipeline {
    agent any

    environment {
        IMAGE_NAME = 'two-tier-flask-app:latest'
        CONTAINER_NAME = 'python-container'
        GIT_URL = 'https://github.com/harshshsh2004/two-tier-flask-app.git'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir() // remove all old files from workspace
            }
        }

        stage('Clone Git Repo') {
            steps {
                echo "Cloning project from GitHub..."
                git branch: 'main', url: '$GIT_URL'
            }
        }

        stage('Configure Jenkins Server') {
            steps {
                echo "Setting permissions..."
                sh ''' chmod 666 /var/run/docker.sock
                   '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Stopping and removing existing container (if any)..."
                sh "docker-compose down || true"

                echo "Running new Docker container..."
                sh "docker-compose up -d"
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "✅ Build and deployment successful!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for errors."
        }
    }
}