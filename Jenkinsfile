pipeline {

    // to denfin node which the pipeline will create on it 
    agent {label "agent-app"}

    stages {
        stage('get code') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/mahmoud254/jenkins_nodejs_example'
            }
        }
        
        stage('CI'){
            steps{
                // to get docker hub credential 
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                
                // to build docker image from node app and push it to my docker hub
                sh '''
                    docker build . -f dockerfile -t mohamedabdou4/final-project-node-app
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push mohamedabdou4/final-project-node-app
                '''
                }
            }   
        }

        stage('CD'){
            steps{
                // to deploy node app as a k8s deployment
                sh ''' 
                    kubectl apply -n node-app-ns -f app-K8s/node-app-dep.yml
                    kubectl apply -n node-app-ns -f app-K8s/node-app-svc.yml
                '''
            }
        }
    }
}
