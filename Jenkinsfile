pipeline {
	agent any
	stages {
		stage ("Git checkout"){
			steps {
				git branch: "master",
					url: "https://github.com/aqulive/PythonVuln.git"
			}
		}
		stage ("Install"){
			steps{
				sh "apt update -y"
				sh "apt install python3-pip -y"
				sh "pip3 install bandit"
				sh "pip3 install safety"
				sh "pip3 install insecure-package"
			}
		}
		stage ("Python Flask Prepare"){
			steps {
				sh "pip3 install -r requirements.txt"
			}

		}
		stage ("Python Bandit Security Scan"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) secfigo/bandit:latest"
				sh "bandit -r /var/jenkins_home/workspace/securitytesting/movie.py > report.txt"
				sh "cat report.txt"
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/movie.py"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/movie.py --json > report.json"
			}
		}					
	}
}
