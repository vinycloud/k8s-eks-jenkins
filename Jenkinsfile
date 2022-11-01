node {

    stage("Git Clone"){

        git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/vinycloud/k8s-eks-jenkins.git'
    }

     stage('Gradle Build') {

       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t fialho-docker-demo .'
        sh 'docker image list'
        sh 'docker tag fialho-docker-demo vinycloud/fialho-docker-demo:fialho-docker-demo'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u vinycloud -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  vinycloud/fialho-docker-demo:fialho-docker-demo'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
    }
} 