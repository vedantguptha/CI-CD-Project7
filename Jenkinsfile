pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }
    stages {
        stage('Pull Code Form Git Hub Repo') {
            steps {
                git 'https://github.com/vedantguptha/CI-CD-Project7.git'
            }
        }
        stage('Build Code') {
            steps {
                 sh 'mvn -Dmaven.test.failure.ignore=true clean package'
            }
        }
    }
}
