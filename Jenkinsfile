pipeline {
    agent any
    environment {
        NEXUS_CREDS = credentials('nexus_creds')
        NEXUS_DOCKER_REPO = 'localhost:8083'
    }

    stages {
       
       stage('Docker Build') {
        
            steps { 
                    echo 'Building docker Image'
                    sh 'docker build -t $NEXUS_DOCKER_REPO'
                }
        }

       stage('Docker Login') {
            steps {
                echo 'Nexus Docker Repository Login'
                script{
                    withCredentials([usernamePassword(credentialsId: 'nexus_creds', usernameVariable: 'USER', passwordVariable: 'PASS' )]){
                       sh ' echo $PASS | docker login -u $USER --password-stdin $NEXUS_DOCKER_REPO'
                    }
                   
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Imgaet to docker hub'
                sh 'docker push $NEXUS_DOCKER_REPO/fakeweb:$BUILD_NUMBER'
            }
        }
    }
}
