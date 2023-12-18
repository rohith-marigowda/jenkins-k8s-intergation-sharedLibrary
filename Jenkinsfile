@Library('sharedLibrary')_

pipeline {

	environment {
        dockerImage = "rohithmarigowda/assignment"
        branchName = "master"
        dockerTag = "ver-${branchName}-${BUILD_NUMBER}"
    }
    
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                gitCheckout('https://github.com/rohith-marigowda/jenkins-k8s-integration.git', 'master', 'github')
            }
        }

        stage('Docker Build') {
            steps {
                dockerImageBuild('$dockerImage', '$dockerTag')
            }
        }

        stage('Docker Push') {
            steps {
                dockerHubImagePush('$dockerImage', '$dockerTag', 'dockerhub')
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                kubernetesEKSDeploy('$dockerImage', '$dockerTag', 'jenkins-k8s-integration-assignment', 'tomcat-with-k8s', 'awscred', 'ap-south-1', 'eks-cluster')
            }
        }

    }
}