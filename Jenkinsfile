pipeline {
    agent any
    tools {
        maven "maven"
    }
    stages {
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        stage('Code Commit') {
            steps {
                git 'https://github.com/vedantguptha/CI-CD-Project7.git'
            }
        }
        stage('Integration Test') {
            steps {
                 sh 'mvn test'
            }
        }
        stage ('Package') {
            steps {
                    sh 'mvn package'
            }  
        }
    }
}
