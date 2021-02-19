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
			steps {
				sh "apt update"
				sh "apt install sudo"
				sh "sudo apt install python3-pip -y"
				sh "pip3 install -r requirements.txt"
			}
		}
		stage("docker"){
			steps{
				sh "sudo apt update"
				sh "sudo apt install apt-transport-https ca-certificates curl software-properties-common -y"
				sh "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"
				sh "sudo add-apt-repository \"deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable\""
				sh "sudo apt update"
				sh "apt-cache policy docker-ce"
				sh "curl -O https://download.docker.com/linux/debian/dists/buster/pool/stable/amd64/containerd.io_1.4.3-1_amd64.deb"
				sh "sudo apt install ./containerd.io_1.4.3-1_amd64.deb"
				sh "sudo apt install docker-ce -y"
				sh "sudo service docker start"
				sh "sudo usermod -aG docker jenkins"
			}
		}
		stage ("Python Bandit Security Scan"){
			steps{
				//sh "bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*"
				sh "dockerd run --rm --volume \$(pwd) secfigo/bandit:latest"
			}
		}
		/*stage ("Dependency Check"){
			steps{
				dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}*/
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/* --json > ./reportsafety.json"
				sh "docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				sh "docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
			}
		}
		stage ("Static Analysis with python-taint"){
			steps{
				sh "docker run --rm --volume \$(pwd) vickyrajagopal/python-taint-docker pyt ."
			}
		}				
	}
}
