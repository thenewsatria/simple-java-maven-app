node {
    def dockerImage = docker.image('maven:3.9.0')

    stage('Checkout') {
        dockerImage.pull()
        dockerImage.inside('-v /root/.m2:/root/.m2') {
            checkout scm
        }
    }

    stage('Build') {
        dockerImage.inside('-v /root/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    stage('Test') {
        dockerImage.inside('-v /root/.m2:/root/.m2') {
            sh 'mvn test'
        }
    }
}