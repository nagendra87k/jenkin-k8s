node{
    stage("Git Clone"){
        git credentialsId: 'GITHUB', url: 'https://github.com/nagenra87k/jenkin-k8s.git'
    }
    stage("MVN Clean Build"){
        def mavenHome = tool name:"Maven-3.6.1", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    stage("Build Docker Images"){
        sh "docker build -t ajayendra/jenkin-k8s ."
    }
    stage("Push Docker Images"){
        withCredentials([string(credentialsId: 'DockerHUB', variable: 'DockerHUB')]) {
             sh "docker login -u ajayendra -p ${DockerHUB}"
        }
        sh "docker push ajayendra/todo-web-application-h2:0.0.1-SNAPSHOT"
    }
    stage('Apply Kubernetes files') {
    withKubeConfig([credentialsId: 'KUBERNETES', serverUrl: 'https://34.66.217.179']) {
      sh 'kubectl apply -f deployment.yaml'
    }
    }
    
   
}
