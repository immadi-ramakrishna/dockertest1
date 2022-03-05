pipeline {
    agent any


    environment {
      DOCKER_TAG = getVersion()
    }

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
            sh 'docker build -t charan2135/pipelinetest2:${DOCKER_TAG} .'
          }
        }
            
        stage('Push Image to Docker Hub') {
          steps {  
            sh 'docker push charan2135/pipelinetest2:${DOCKER_TAG}'
          }
        }
            
        stage('Deploy to Docker Host') {
          steps {
            sh '''if [  "$(docker -H tcp://10.0.0.250:2375 ps -q -f name=webapp1)" ]; then
                  # cleanup
                  docker -H tcp://10.0.0.250:2375 stop webapp1
                  else
                  echo \'create webapp\'
                  fi'''
            sh 'docker -H tcp://10.0.0.250:2375 run --rm -dit --name webapp1 --hostname webapp1 -p 9000:80 charan2135/pipelinetest2:${DOCKER_TAG}'
          } 
        }
            
        stage('Check webapp1 Reachablity') {
          steps {
            sh 'sleep 10s'
            sh 'curl http://ec2-13-126-54-65.ap-south-1.compute.amazonaws.com:9000'
          } 
        }
    }
}



def getVersion() {
  def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
  return commitHash
}
