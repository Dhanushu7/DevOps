pipeline {
    agent any
    tools {
        gradle 'gradle-7.4.2'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'git@github.com:Dhanushu7/DevOps.git', branch: 'develop', credentialsId: 'github-ssh'
            }
        }
        stage('Build') {
            steps {
                sh 'gradle clean build'
            }
        }
        stage('Test') {
            steps {
                sh 'gradle test'
            }
        }
        stage('Publish') {
            steps {
                rtServer (
                    id: "artifactory",
                    url: "http://localhost:8081/artifactory",
                    credentialsId: "artifactory-cred"
                )
                rtGradleDeployer (
                    id: "gradle-deployer",
                    serverId: "artifactory",
                    repoKey: "libs-release-local"
                )
                sh 'gradle artifactoryPublish'
            }
        }
    }
}
