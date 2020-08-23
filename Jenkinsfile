node{
    stage("Git Clone"){
        git credentialsId: 'GITHUBID', url: 'https://github.com/nagenra87k/jenkin-k8s.git'
    }
    stage("MVN Clean Build"){
        def mavenHome = tool name:"Maven-3.6.1", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    stage("Build Docker Images"){
        sh "docker build -t nagenra87k/jenkin-k8s ."
    }
    stage("Push Docker Images"){
        withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENCIAL', variable: 'DOCKER_HUB_CREDENCIAL')]) {
             sh "docker login -u nagendra87k -p ${DOCKER_HUB_CREDENCIAL}"
        }
        sh "docker push nagendra87k/todo-web-application-h2:0.0.1-SNAPSHOT"
    }
   
    stage('Deploy Application on K8s') {
        container('kubectl'){
            withKubeConfig(
            [
            serverUrl: "104.198.161.8",
            clusterName: "cluster-1"]
            )
            {
                sh("kubectl apply -f deployment.yaml")
            }
        }
    }
}
