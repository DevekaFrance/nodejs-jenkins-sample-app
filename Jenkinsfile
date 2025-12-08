pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "jenkins-demo-app"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            // TODO: Récupérer le code source
        }
        
        stage('Install Dependencies') {
            // TODO: Installer les dépendances
        }
        
        stage('Run Tests') {
            // TODO: Lancer les tests
        }
        
        stage('Build Docker Image') {
            // TODO: Construire l'image Docker
        }
        
        stage('Deploy') {
            // TODO: Déployer le conteneur
            // Arrêter l'ancien conteneur s'il existe 
            // Démarrer le nouveau conteneur avec la nouvelle version
        }
    }
    
    post {
        // TODO: Partie bonus
    }
}