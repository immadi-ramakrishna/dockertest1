pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {  
            sh 'rm -rf /var/lib/jenkins/workspace/pipeline2/dcokertest1'
            sh 'git clone https://github.com/ck2135/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2/dockertest1'
            sh ' cp /var/lib/jenkins/workspace/pipeline2/dockertest1/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t charan2135/pipelinetest:05-2021 .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
            sh 'docker push charan2135/pipelinetest:05-2021'
            }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh 'docker -H tcp://10.50.1.201:2375 run --rm --name webapp1 --hostname webapp1 -p 9000:80 charan2135/pipelinetest:05-2021'
            }
        }

        stage('Check WebApp Reachability') {
          steps {
            sh 'sleep 10s'
            sh 'curl ec2-54-198-31-70.compute-1.amazonaws.com:9000'
            }
        }
    }
}
