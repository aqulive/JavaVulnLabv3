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
				sh "bandit -r /var/jenkins_home/workspace/securitytesting/bad/* > report.txt"
				sh "cat report.txt"
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy.py"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy.py --json > reportvulnpy.json"
				sh "cat ./reportvulnpy.json"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy-ssl.py"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy-ssl.py --json > reportvulnpy-ssl.json"
				sh "cat ./reportvulnpy-ssl.json"
			}
		}					
	}
}
