pipeline {
    agent {
        docker { image 'reolwe/jan:latest'
        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'}
    }
    stages {
        stage('git') {
            steps {
               git 'https://github.com/reolwe/origin555.git'
            }
        }
        stage('build') {
            steps {sh 'ls -l &&  mvn package'}
        }
        stage('docker image') {
            steps {
                sh 'docker login -u reolwe -p dckr_pat_ZydUH_tEynqAgDlLQSQhZGE9tPE'
                sh 'docker build --tag=reolwe/tomcatprod .&& docker push reolwe/tomcatprod'
            }
        }
        stage('run docker on prod') {
            agent any
            steps {
                sshagent(credentials : ['677ab073-43d4-4184-811c-e4f491fbc066']) {
            sh 'ssh -o StrictHostKeyChecking=no root@84.252.139.103 uptime'

            sh 'docker pull reolwe/tomcatprod'
            sh 'docker run -d -p 8080:8080 reolwe/tomcatprod'
        }
            }
        }

    }
}