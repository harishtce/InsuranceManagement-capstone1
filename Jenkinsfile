pipeline {
    agent any
    environment {
	MAVEN_HOME = tool name: 'mavenHome', type: 'maven'
	MAVEN_CMD = "${MAVEN_HOME}/bin/mvn"
	TAG = "3.0"
	DOCKER_HUB_USER = "harishtce"
	CONTAINER_NAME = "insure-me"
	HTTP_PORT = "8081"
    }

    stages {
        stage('Code Checkout') {
            steps {
                echo 'Checking out code..'
		checkout scm
            }
        }
        stage('Maven Build') {
            steps {
                echo 'Building Artifacts..'
		sh "$MAVEN_CMD clean package"
            }
        }
        stage('Docker Image Build') {
            steps {
                echo 'Building Docker Image....'
		sh "docker build -t $DOCKER_HUB_USER/$CONTAINER_NAME:$TAG --pull --no-cache ."
            }
        }
	stage('Docker Image Scan') {
            steps {
                echo 'Scanning Docker Image for Vulnerabilities....'
		sh "docker build -t $DOCKER_HUB_USER/$CONTAINER_NAME:$TAG ."
            }
        }
	stage('Publishing Image to Docker Hub') {
            steps {
                echo 'Pushing the docker image to DockerHub'
        	withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
			sh "docker login -u $dockerUser -p $dockerPassword"
			sh "docker push $dockerUser/$CONTAINER_NAME:$TAG"
			echo "Image push complete"
		}
            }
        }
	stage('Docker Container Deployment') {
            steps {
                sh "docker rm $CONTAINER_NAME -f"
		sh "docker pull $DOCKER_HUB_USER/$CONTAINER_NAME:$ATG"
		sh "docker run -d --rm -p $HTTP_PORT:$HTTP_PORT --name $CONTAINER_NAME $DOCKER_HUB_USER/$CONTAINER_NAME:$ATG"
		echo "Application started on port: ${HTTP_PORT} (http)"
            }
        }
    }
}
