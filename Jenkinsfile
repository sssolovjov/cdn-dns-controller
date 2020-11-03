pipeline {
	environment {
		repository = "https://github.com/sssolovjov/cdn-dns-controller.git"
		repositoryCredentials = "sssolovjov"
		imageName = "cdn-dns-controller"
	}
	agent any
	options {
		timeout (time: 1, unit: 'HOUR')
	}
	stages {
		stage('git clone') {
			steps {
				git credentialsId: ${repositoryCredentials}
					url: ${repository}
			}
		}
		stage('tests') {
			steps {
				sh 'python3 -m pytest tests/'
				//bat
			}
		}
		stage ('create docker image') {
			steps{
				sh 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
			}
		}
		stage ('run docker') {
			steps {
				sh 'docker run -it -d -p 5000:5000 -rm --name cdn-dns-controller cdn-dns-controller'
			}
		}
		stage ('sanity test') {
			steps {
				sh 'curl localhost:5000/'
			}
		}
		stage ('cleanup') {
			steps {
				sh 'docker rm -f cdn-dns-controller'
			}
		}
	}
}