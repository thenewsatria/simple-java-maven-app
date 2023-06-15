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

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?'
    }

    stage('Deploy') {
        dockerImage.inside('-v /root/.m2:/root/.m2') {
            sh './jenkins/scripts/deliver.sh'
            
            // sebagai syarat
            sleep(time: 1, unit: 'MINUTES')
        }
    }
}