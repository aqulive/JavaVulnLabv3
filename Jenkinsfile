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
				sh "sudo apt install python3-pip"
				sh "pip3 install -r requirements.txt"
			}
		}
		/*stage ("Python Bandit Security Scan"){
			steps{
				sh "bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*"
			}
		}*/
		stage("docker"){
			steps{
				sh "sudo apt update"
				sh "sudo apt install apt-transport-https ca-certificates curl software-properties-common -y"
				sh "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"
				sh "sudo add-apt-repository \"deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable\""
				sh "sudo apt update"
				sh "apt-cache policy docker-ce"
				sh "sudo apt install docker-ce -y"
				sh "sudo systemctl status docker"
				sh "sudo usermod -aG docker jenkins"
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
		/*stage ("Dependency Check with Python Safety"){
			steps{
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/* --json > ./reportsafety.json"
			}
		}*/				
	}
}
