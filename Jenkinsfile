node {

    stage 'Checkout'
    git "https://github.com/anqingxu/petclinic.git"

    stage 'Build application war file'
    // Build petclinic in a Maven3+JDK8 Docker container
    docker.image('maven:3-jdk-8').inside('-v /.m2:/root/.m2') {
        sh 'mvn -B package -DskipTests'
    }

    stage 'Build application Docker image'
    def appImg = docker.build("anqingxu/petclinic:v1.0")

    stage 'Push to GCR'
    docker.withRegistry('', 'dockerhub') {
        appImg.push();
    }

}
