pipeline {

    agent none

    environment {
      DOCKERHUB_CREDENTIALS = credentials('c274a25c-2d8a-468c-92c2-f9815df69b19')
      DOCKERHUB_BACKEND_REPOSITORY = 'aldorama/proyecto1-backend-jenkins'
    }
  
    stages {
        stage('Instalar dependencias de backend...') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
        
            steps {
                echo 'Instalando dependencias de nodejs'
                sh 'npm install'
            }
        }

        stage('Ejecutar pruebas unitarias') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
        
            steps {
                echo 'Ejecutando tests'
                sh 'npm run test'
            }
        }

        stage('Publicar imagen en Dockerhub') {
            agent {
                docker {
                    image 'docker:latest'
                }
            }
        
            steps {
                echo 'Setear credenciales de dockerhub y pushear...'
                
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker build -t $DOCKERHUB_BACKEND_REPOSITORY:latest .
                docker push $DOCKERHUB_BACKEND_REPOSITORY:latest
                docker logout
                '''
            }
        }
    }
}