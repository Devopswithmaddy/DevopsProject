pipeline {
    agent any
    
    // environment {
    //     DOCKER_USER = credentials('DOCKER_USER')
    //     DOCKER_PASSWD = credentials('DOCKER_PASSWD')
    //     OC_USER = credentials('OC_USER')
    //     OC_PASSWD = credentials('OC_PASSWD')
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout your code from the specified GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: 'master']], userRemoteConfigs: [[url: 'https://github.com/devopswithcloud/spring-petclinic.git']]])
            }
        }
        
        stage('Docker Login') {
            steps {
                // Login to Docker Hub
                sh "docker login -u madhavkrishna118@gmail.com -p 9010438019"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh "docker build --no-cache --force-rm -t spring:jenkinsimage ."
            }
        }
        
        stage('Tag Docker Image') {
            steps {
                // Tag the Docker image
                sh "docker tag spring:jenkinsimage madhavkrishna118/ubuntu:springjenkins"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                // Push Docker image to Docker Hub
                sh "docker push madhavkrishna118/ubuntu:springjenkins"
            }
        }
        
        stage('OpenShift Login') {
            steps {
                // Login to OpenShift
                sh "oc login https://api.prod.madhavopenshift.shop:6443 -u ${OC_USER} -p ${OC_PASSWD} --insecure-skip-tls-verify=true"
            }
        }
        
        stage('Select Project') {
            steps {
                // Shift to the desired OpenShift project
                sh "oc project java-springboot"
            }
        }
        
        stage('Deploy to OpenShift') {
            steps {
                // Deploy to OpenShift
                sh "oc rollout latest dc/java-springboot"
            }
        }
    }
}
