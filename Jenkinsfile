node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-minikube"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName
    env.BUILD_TAG=tag

    stage "Build"
    
        sh "docker build -t ${imageName} -f apps/hello-minikube/Dockerfile apps/hello-minikube"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        kubernetesDeploy configs: "apps/${appName}/k8s/*.yaml", kubeconfigId: 'hello_kubeconfig'

}
