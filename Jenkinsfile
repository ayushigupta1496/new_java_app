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
				sh 'sudo docker build --tag java-app:$BUILD_TAG_$BUILD_NAME .'
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
		
	}	
}	
