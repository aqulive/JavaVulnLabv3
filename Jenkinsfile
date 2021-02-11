pipeline {
    agent any

    options{
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    node {
        stage('SCM') {
            git 'https://github.com/aqulive/JavaVulnLabv3.git'
        }
        stage('SonarQube analysis') {
            withSonarQubeEnv(credentialsId: 'b1973be2-ddb5-42c7-b02a-465099ea237d', installationName: 'My SonarQube Server') { // You can override the credential to be used
            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
            }
        }
    }
    node {
        stage ('Test'){
            echo 'Hey'
        }
    }
}
