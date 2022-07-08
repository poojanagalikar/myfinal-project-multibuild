pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "m3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', credentialsId: 'my-token', url: 'https://github.com/poojanagalikar/movieapp-ansible-tomcat.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Deploy to aks') {
            steps {
                script {
                        kubeconfig(credentialsId: 'k8scred', serverUrl: 'https://aksdemo1-dns-e153705a.hcp.eastus.azmk8s.io:443') {
                        sh 'kubectl apply -f mypv.yml'
                        sh 'kubectl apply -f mypvc.yml'
                        sh 'kubectl apply -f mysql-secret.yml'
                        sh 'kubectl apply -f configmap.yml'
                        sh 'kubectl apply -f mysql-deployement.yml'
                        sh 'kubectl apply -f mysql-service.yml'
                        sh 'kubectl apply -f moviemgmt.yml'
                    }
                }
            }
        }
    }
}
