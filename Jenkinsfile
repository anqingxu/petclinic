pipeline {
  agent {
    kubernetes {
      label 'petclinic'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  environment {
    registry = '632912596221.dkr.ecr.us-east-1.amazonaws.com'
    registryCredential = 'ecr_id'
    dockerImage = ''
  }
  stages {
    stage('Build') {
      steps {  // no container directive is needed as the maven container is the default
        sh "mvn -B package -DskipTests"
      }
    }
    stage('Build Docker Image') {
      steps {
        container('docker') {
          //withDockerRegistry([ credentialsId: "ecr:us-east-1:" + registryCredential, url: "" ]) {
          //  sh "docker build -t anqingxu/petclinic:v1.0.0 ."
          //  sh "docker push anqingxu/petclinic:v1.0.0"
          dockerImage = docker.build registry + "/anqingxu/petclinic:v1.0.0"
          docker.withRegistry("https://" + registry, "ecr:us-east-1:" + registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
  }
}
//https://stackoverflow.com/questions/59084989/push-to-ecr-from-jenkins-pipeline
