node {
    def application = "mynginx"
    def dockerhubaccountid = "mandheit"
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
        app.push()
        app.push("latest")
    }
    }

    stage('Deploy_stop_container') {
        sh ("docker stop ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }
    stage('Deploy_remove_container') {
        sh ("docker rm ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }
    stage('Deploy') {
        sh ("docker run -d -p 80:80 ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Remove old images') {
        // remove old docker images
        sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
