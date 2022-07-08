pipeline {
    environment {
        registry = "40.88.1.154:8085/movieappnew"
        registryCredential = 'nexushub'
        dockerImage = ''
    }
    agent any 
    // agent is where my pipeline will be eexecuted
    tools {
        //install the maven version configured as m2 and add it to the path
        maven "m3"
    }
    stages {
        stage('pull from scm') {
            steps {
            git branch: 'main', credentialsId: 'my-token', url: 'https://github.com/poojanagalikar/movie-app.git'
            }
        }
         stage('build it') {
            steps {
            sh 'mvn clean package'
            }
        }
        stage('docker image') {
            steps {
                script {
                  dockerImage=docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                  docker.withRegistry('http://40.88.1.154:8085',registryCredential) {
                      dockerImage.push()
                  }
                }
            }
        }
    }
}
