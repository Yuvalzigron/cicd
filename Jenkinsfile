pipeline{
    agent{
        kubernetes{
            yamlFile 'build-pod.yaml'
            defaultContainer 'ez-docker-helm-build'
        }
    }

    environment{
        DOCKER_IMAGE = 'yuvalzigron/jenkins_images'
        GITHUB_API_URL = 'https://api.github.com'
        GITHUB_REPO = 'yuvalzigron/cicd'
        GITHUB_TOKEN = credentials('github-creds')
    }

    stages{
        stage("Checkout code"){
            steps {
                checkout scm
            }
        }

        stage("Build docker image"){
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest", "--no-cache .")
                }
            }
        }

        stage('Push Docker image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}