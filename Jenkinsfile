pipeline {
    agent any
    tools {
        nodejs 'nodejs'
        dockerTool 'docker'
    }
    stages {
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/hatemyakoubi/todo-devops'
            }
        }
        stage("Build") {
            steps {
                script {
                    docker.build("hatemyakoubi/todo-devops:${env.BUILD_NUMBER}") 
                }
            }
        }
        
    stages {
        stage('Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit'
            }
        }
    }
        stage("Push") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') { 
                        docker.image("hatemyakoubi/todo-devops:${env.BUILD_NUMBER}").push() 
                        docker.image("hatemyakoubi/todo-devops:${env.BUILD_NUMBER}").push("latest")
                    }
                }
            }
    }

        stage('Dependency Check') {
            steps {
                script {
                   
                    dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit'
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "kubectl apply -f k8s/"
            }
        }
    }
}
