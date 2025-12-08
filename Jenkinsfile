pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "jenkins-demo-app"
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Récupérer la branche Docker du dépôt
                git branch: 'docker', url: 'https://github.com/Lun0xxx/nodejs-jenkins-sample-app.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        
        stage('Stop Previous Container') {
            steps {
                script {
                    // Arrêter l'ancien conteneur s'il existe
                    sh 'sudo docker ps -q -f "name=${DOCKER_IMAGE}" | grep -q . && docker stop ${DOCKER_IMAGE} || true'
                    // Supprimer l'ancien conteneur s'il existe
                    sh 'sudo docker ps -a -q -f "name=${DOCKER_IMAGE}" | grep -q . && docker rm ${DOCKER_IMAGE} || true'
                    // Supprimer l'ancienne image Docker si elle existe
                    sh 'sudo docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
                }
            }
        }

	stage('Build Docker Image') {
            steps {
                script {
                    sh 'sudo docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
                }
            }
	}

	stage('Deploy') {
	    steps {
		script {
		    // Démarrer un nouveau conteneur avec la nouvelle image
                    sh 'sudo docker run -d --name ${DOCKER_IMAGE} ${DOCKER_IMAGE}:${DOCKER_TAG}'
		}
	    }
	}		    
    }
    
    post {
        always {
            echo 'Pipeline terminé, nettoyage des ressources.'
        }
        success {
            echo 'Le pipeline s\'est terminé avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
