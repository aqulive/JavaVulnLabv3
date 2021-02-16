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
				sh "apt install sudo"
				//sh "curl -fsSL https://get.docker.com -o get-docker.sh"
				//sh "sh get-docker.sh"
				//sh "sudo service docker stop"
				sh "sudo nohup docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock &"
				sh "sudo service docker start"
				sh "sudo usermod -aG docker jenkins"
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
				sh "sudo docker run --rm --volume \$(pwd) secfigo/bandit:latest"
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
			}
		}
		stage ("Static Analysis with python-taint"){
			steps{
				sh "sudo docker run --rm --volume \$(pwd) vickyrajagopal/python-taint-docker pyt ."
			}
		}					
	}
}
