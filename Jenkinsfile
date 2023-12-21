@Library('sharedLibrary')_

pipeline {
	environment {
	branchName = sh(script: 'echo $BRANCH_NAME | sed "s#/#-#"', returnStdout: true).trim()
	gitCommit = "${GIT_COMMIT[0..6]}"
	dockerImage = "rohithmarigowda/assignment"
	dockerTag = "${branchName}-${gitCommit}-${BUILD_NUMBER}"
    }
    
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                gitCheckout('https://github.com/rohith-marigowda/jenkins-k8s-intergation-sharedLibrary.git', 'master', 'github')
            }
        }

	stage('Run Unit and Integration tests'){
  	    steps {
		echo 'In this step run unit and integration tests'
	    }
	 }

	stage('SonarQube analysis') {
        steps{
        withSonarQubeEnv('sonarqube-9.7.1') { 
        sh 'echo execute below command for sonar code analysis'
		//sh "mvn sonar:sonar"
    }
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
                //kubernetesEKSDeploy('$dockerImage', '$dockerTag', 'jenkins-k8s-integration-assignment', 'tomcat-with-k8s', 'awscred', 'ap-south-1', 'eks-cluster')
		kubernetesHelmDeploy('$dockerImage', '$dockerTag', 'tomcatdeploy')
            }
        }

    }
}
