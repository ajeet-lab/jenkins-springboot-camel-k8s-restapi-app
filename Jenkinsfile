pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage("build maven"){
           steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ajeet-lab/jenkins-springboot-camel-k8s-restapi-app']])
                sh 'mvn clean install'
           }
        }
        stage("build docker image"){
           steps{
                sh 'docker build -t ajeet9415/springboot-camel-app:02 .'
           }
        }
        stage("docker login & push image to dockerhub"){
            steps{
               script{
                   withCredentials([string(credentialsId: 'dockerhub-pwds', variable: 'dockerhubpwd3')]) {
                    sh 'docker login -u ajeet9415k@gmail.com -p ${dockerhubpwd3}'
                    }
                    sh 'docker push ajeet9415/springboot-camel-app:02'
                }
            }

        }
        stage("deploy to k8s"){
           steps{
                kubernetesDeploy (configs: 'deployment.yml', kubeconfigId: 'kube8configpwd2')
           }
        }
        stage("expose the service"){
           steps{
                kubernetesDeploy (configs: 'service.yml', kubeconfigId: 'kube8configpwd2')
           }
        }
    }
}