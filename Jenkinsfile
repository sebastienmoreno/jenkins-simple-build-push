node {

  stage('Checkout project') { 
    git 'https://github.com/sebastienmoreno/jenkins-simple-build-push.git'
  }

  stage('Build App') {
    docker.image('maven:3.6-jdk-8-alpine').inside('--network ci') {
      sh 'mvn clean install'
    }
  }

  stage('build docker image'){
    sh 'docker build -t simple-springboot-app .'
  }

  stage('push image to dockerhub') {
    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: "dockerhub", usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
      sh 'docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PASSWORD'
      sh 'docker tag simple-springboot-app $DOCKER_HUB_USER/simple-springboot-app:latest'
      sh 'docker push $DOCKER_HUB_USER/simple-springboot-app:latest'
    }
  }
}

