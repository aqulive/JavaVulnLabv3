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
				//sh "bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*"
				 sh ''' @echo off
                        bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*" 
                        IF %ERRORLEVEL% EQU 1 (exit /B 0) ELSE (exit /B 1)'''
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				//sh "sudo docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
				//sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/*"
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/* --json > ./reportsafety.json"
				//sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy-ssl.py"
				//sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/vulpy-ssl.py --json > reportvulnpy-ssl.json"
			}
		}					
	}
}
