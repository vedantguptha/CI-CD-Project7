pipeline {
    agent any
    tools {
        maven "maven"
    }
    environment {
              APP_NAME = "loginregisterapp"
              DOCKER_USER = "vedantdevops"
              MAJOR_VERSION = "1.0"
              IMAGE_TAG = "${MAJOR_VERSION}.${BUILD_NUMBER}"
              NEW_DOCKER_IMAGE = "${DOCKER_USER}" + "/" + "${APP_NAME}:${IMAGE_TAG}" 
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
                sh " aws s3 cp $WORKSPACE/target/*.war s3://b90-artifactory/${APP_NAME}-${IMAGE_TAG}.war "
            }  
        }
        stage ('Build Docker Iamge') {
            steps {
                sh ''' cd $WORKSPACE ''' 
                sh " docker build -t ${NEW_DOCKER_IMAGE} . "
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
        stage ('Git Checkout') {
            steps {
                build job: 'merge', parameters: [string(name: 'release_version', value:  String.valueOf(BUILD_NUMBER)  )]
            }  
        }
    }
}
