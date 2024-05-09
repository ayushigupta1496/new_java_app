pipeline {
	agent {
		label "ubuntu-slave"
	}
	stages {
		stage ("Pulling the code from SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/ayushigupta1496/new_java_app.git'
			}
		}
		stage ("Build the code") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
                                sh 'sudo mvn clean package'
			}
		}
		stage ("Create docker image"){
			steps {
				sh 'sudo docker build -t java-app:$BUILD_TAG .'
				sh 'sudo docker tag java-app:$BUILD_TAG ayushigupta1496/java-app:$BUILD_TAG'
			}
		}
		stage ("Push on Docker-Hub"){
			steps {
				withCredentials([string(credentialsId: 'Docker_pass_ID', variable: 'docker_hub_pass_var')]) {
					sh 'sudo docker login -u ayushigupta1496 -p ${docker_hub_pass_var}'
					sh 'sudo docker push  ayushigupta1496/java-app:$BUILD_TAG' 		
				}
			}
		}
		stage (" Testing the pipeline" ){
				steps {
					sh 'sudo docker run -dit --name java-test$BUILD_TAG -p 8083:8080 java-app:$BUILD_TAG'
				}
			}
	
stage ("QAT Testing"){
			steps {
				retry(5) {
					script {
						sh 'sudo curl --silent http://13.201.115.201:8083/java-web-app/ | grep -i -E "(india|sr)"'
					}
				}
			}
		}
		stage ("Approval from QAT"){
			steps {
				script {
					Boolean userInput = input(id: 'Proceed1', message: 'Do you want to Promote this build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                				echo 'userInput: ' + userInput
				}
			}
		}
		stage("Deployment"){
		      steps{
		           script{
			        kubernetesDeploy configs: '', kubeConfig: [path: 'https://github.com/ayushigupta1496/new_java_app/blob/main/deploymentservice.yaml'], kubeconfigId: 'kubernetesconfigkey'
				}
				}
				}
				}
				}
