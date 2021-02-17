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
				sh "pip3 install python-taint"
			}
		}
		stage ("Python Flask Prepare"){
			steps {
				sh "pip3 install -r requirements.txt"
			}

		}
		stage ("Unit Test"){
			steps{
				sh "python3 test_basic.py"
			}
		}
		stage ("Python Bandit Security Scan"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) secfigo/bandit:latest"
				sh "bandit -r \$(pwd)"
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
				sh "safety check -r \$(pwd)"
				sh "safety check -r \$(pwd) --json > report.json"
			}
		}
		stage ("Static Analysis with python-taint"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) vickyrajagopal/python-taint-docker pyt ."
				sh "python3 -m pyt \$(pwd)"
			}
		}					
	}
}
