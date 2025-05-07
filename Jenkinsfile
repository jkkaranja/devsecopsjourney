pipeline {
    agent any
 
    triggers {
        pollSCM 'H/5 * * * *'
    }
 
    environment {
        DOCKERHUB_CREDENTIALS = credentials ('John-dockerhub')
    }
 
    stages {
        stage('Setup Node') {
            steps {
                echo "Checking node version"
                sh "node --version"
                sh "npm --version"
               
            }
        }
         stage('Install Dependencies') {
            steps {
                echo "Installing dependencies.."
                sh 'npm install'
               
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh 'npm run test:unit'
            }
        }
         stage('Build') {
         steps {
               echo 'Building....'
             sh 'npm run build'
            archiveArtifacts artifacts: 'dist/**', fingerprint: true
         }
         }
       stage('Building') {
            steps {
                echo 'Building....'
                sh 'docker build -t jkkaranja/devsecopsjourney:$BUILD_ID .'
               
            }
        }
      stage('login') {
          steps {
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
      }
      stage ('push'){
          steps {
            sh 'docker push jkkaranja/devsecopsjourney:$BUILD_ID'
           }  
         }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
