pipeline {
    agent {
        label "built-in"
    }
    
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        // Initialize global variables here
        APP_NAME = "arkamimage"
        DOCKER_USER = "arkamabdelbari"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = '1.0'   
    }   

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
    
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ArkamAbdelbari/pipeproject'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Push to Registry") {
            steps {
                script {
                    // Build Docker image
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image = docker.build("${IMAGE_NAME}")
                    }

                    // Push Docker image with specified tag and 'latest'
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
//dckr_pat_bpdQxhwZS4ELNRUBQ2JtzLqgGJU