pipeline {
        environment {
        registry = 'harbor-core.az.tillman.wtf/tillmanwtf/pacoloco'
        registryCredential = 'harbor-core.az.tillman.wtf'
        dockerImage = ''
    }
    agent {
        kubernetes {
            yamlFile 'KubernetesBuildPod.yaml'
        }
    }
    stages {
        stage('Git Clone') {
            steps {
                git branch: 'master', credentialsId: '1b8dfbf1-2419-4537-a4af-016d1309db34', url: 'https://github.com/BrianTillman/pacoloco.git'
            }
        }
        stage('Build Container Image') {
            steps {
                container('docker') {
                    script {
                        dockerImage = docker.build("${registry}:${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Publish Container Image') {
            steps {
                container('docker') {
                    script {
                        withDockerRegistry(credentialsId: registryCredential, url: "https://${registry}") {
                            dockerImage.push("latest")
                        }
                    }
                }
            }
        }
    }
}