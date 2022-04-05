node{
    stage('Git Checkout'){
        git branch: 'main', credentialsId: 'git-private-key', poll: false, url: 'git@github.com:andreimoldovan23/BeerEurekaServer.git'
    }

    stage('Build Project'){
        sh 'sudo mvn clean install'
    }

    stage('Build Docker Image'){
        sh 'sudo docker build -t moldoandrei/beer-eureka-server .'
    }

    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
            sh "sudo docker login -u moldoandrei -p ${dockerHubPwd}"
        }
        sh 'sudo docker push moldoandrei/beer-eureka-server'
    }

    stage('Run Container'){
        sh 'docker rm --force eurekaserver'
        sh 'docker run --name eurekaserver -p 8761:8761 moldoandrei/beer-eureka-server'
    }
}