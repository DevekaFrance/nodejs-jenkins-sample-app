pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "jenkins-demo-app"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Récupère le code depuis la branche docker du dépôt
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'docker']],
                    userRemoteConfigs: [[url: 'https://github.com/DevekaFrance/nodejs-jenkins-sampleapp.git']]
                ])
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installation des dépendances Node.js...'
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Lancement des tests...'
                sh 'npm test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Construction de l'image Docker: ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    // Construit l'image à partir du Dockerfile du dépôt
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    echo 'Déploiement du conteneur...'
                    
                    // Arrête et supprime l'ancien conteneur s'il existe
                    sh '''
                        docker stop jenkins-app || true
                        docker rm jenkins-app || true
                    '''
                    
                    // Lance le nouveau conteneur sur le port 3000
                    sh "docker run -d --name jenkins-app -p 3000:3000 ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline terminé (build #${BUILD_NUMBER})"
        }
        success {
            echo "✅ Déploiement réussi sur http://localhost:3000"
        }
        failure {
            echo "❌ Échec du pipeline"
        }
    }
}
