pipeline {

    def mavenHome, mavenCMD, tag, dockerHubUser, containerName, httpPort = ""

    agent any

	stages{

	

		stage('Prepare Environment'){

			steps{

				echo 'Initialize Environment'

				agent any

				mavenHome = tool name: 'mavenHome' , type: 'maven'

				mavenCMD = "${mavenHome}/bin/mvn"

				tag="3.0"

				dockerHubUser="harishtce"

				containerName="insure-me"

				httpPort="8081"

			}

		}



		stage('Code Checkout'){

		    steps{

				try{

					checkout scm

				}

				catch(Exception e){

					echo 'Exception occured in Git Code Checkout Stage'

					currentBuild.result = "FAILURE"

					emailext body: '''Dear All,

					The Jenkins job ${JOB_NAME} has been failed. Request you to please have a look at it immediately by clicking on the below link.

					${BUILD_URL}''', subject: 'Job ${JOB_NAME} ${BUILD_NUMBER} is failed', to: 'harishtce@gmail.com'

				}

			}

		}

	}

}
