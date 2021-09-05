node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    tag = "latest"
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    //registryHost = "gcr.io/robotics-222114/hello-kenzan"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName
    env.BUILD_TAG=tag
    //stage('Initialize') {
    //    def dockerHome = tool 'myDocker'
    //    env.PATH = "${dockerHome}/bin:${env.PATH}"
    //}
    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"
        sh "kubectl config current-context"
        sh "kubectl apply -f applications/${appName}/k8s/deployment.yaml" 
        //kubernetesDeploy configs: "applications/${appName}/k8s/deployment.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
