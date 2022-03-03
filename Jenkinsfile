pipline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1/'
            sh 'git clone https://github.com/ck2135/dockertest1.git'
          }
        }
        
        stage('Build Docker Image') {
          steps {  
            sh 'cd /var/lib/jenkins/workspace/pipeline2/dockertest1'
            sh 'cp /var/lib/jenkins/workspace/pipeline2/dockertest1/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t charan2135/pipelinetest2:v1 .'
          }
        }
            
        stage('Push Image to Docker Hub') {
          steps {  
            sh 'docker push charan2135/pipelinetest:v1'
          }
        }
            
        stage('Deploy to Docker Host') {
          steps {
            sh 'docker -H tcp:/10.0.0.200:2375 stop webapp1'
            sh 'docker -H tcp:/10.0.0.200:2375 run --rm -dit --name webapp1 --hostname webapp1 -p 9000:80 charan2135/pipelinetest2:v1'
           }
        }
            
        stage('Check WebApp Reachablity') {
          steps {
            sh 'sleep 10s'
            sh 'curl http://ec2-13-127-213-130.ap-south-1.compute.amazonaws.com:9000'
           } 
        }
    }
}
