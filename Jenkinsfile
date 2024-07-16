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
        stage ('Artifactory') {
            steps {
                sh 'aws s3 cp $WORKSPACE/target/*.war s3://b90-artifactory/LoginRegisterApp-$BUILD_NUMBER.war'
            }  
        }
        stage ('Build Docker Iamge') {
            steps {
                sh ''' cd $WORKSPACE ''' 
                sh ''' docker build -t vedantdevops/loginregisterapp:$BUILD_NUMBER . ''' 
            }  
        }
        stage ('Registery Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh ' docker login -u $USERNAME -p $PASSWORD '
                }
            }  
        }
         stage ('Push Docker Image') {
            steps {
                sh ''' docker push vedantdevops/loginregisterapp:$BUILD_NUMBER ''' 
            }  
        }
    }
}
