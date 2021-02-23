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
				sh "sudo apt update"
				sh "sudo apt install python3-pip -y"
				sh "pip3 install -r requirements.txt"
			}
		}
		stage ("Python Bandit Security Scan"){
			steps{
				//sh "bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*"
				sh "sudo docker run --rm --volume \$(pwd) secfigo/bandit:latest"
			}
		}
		stage ("Dependency Check"){
			steps{
				dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/* --json > ./reportsafety.json"
				sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
			}
		}				
	}
}
