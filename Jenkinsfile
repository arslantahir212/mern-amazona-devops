pipeline
 agent any
 stages{
   stage('Code') {
     steps{
      git branch: 'main', url: 'https://github.com/arslantahir212/mern-amazona-devops.git'
     }
   }
   stage('Build Backend Image') {
      steps {
        dir('backend') {
          sh 'docker build -t arslanlinux/mern-backend:latest.'
        } 
      } 
   }
   stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh 'docker build -t arslanlinux/mern-frontend:latest .'
                }
            }
     }
   stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                sh 'docker push arslanlinux/mern-backend:latest'
            }
        }

        stage('Push Frontend Image') {
            steps {
                sh 'docker push arslanlinux/mern-frontend:latest'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker compose down
                    docker compose up -d
                '''
            }
        }
  
    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            sh 'docker ps'
            sh 'docker images'
        }
    }
}
