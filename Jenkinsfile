pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                //checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rohith-marigowda/k8s_jenkins_integration']]])
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rohith-marigowda/k8s_jenkins_integration']])
                sh 'cd /var/lib/jenkins/workspace/jenkins_k8s_integration'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t rohithmarigowda/assignment .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   sh 'docker login -u {enterYourUsername} -p {enterYourPassword}'
                    sh 'docker push rohithmarigowda/assignment'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
