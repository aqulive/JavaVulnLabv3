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
				sh "pip3 install -r requirements.txt"
			}

		}
		/*stage ("Python Bandit Security Scan"){
			steps{
				sh "bandit -f json -o ./reportbandit.json -r /var/jenkins_home/workspace/securitytesting/bad/*"
			}
		}*/
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
		/*stage ("Dependency Check with Python Safety"){
			steps{
				sh "safety check -r /var/jenkins_home/workspace/securitytesting/bad/* --json > ./reportsafety.json"
			}
		}*/					
	}
}
