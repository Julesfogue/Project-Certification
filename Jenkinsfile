pipeline {
    agent none 
    stages {
        stage('Stage 1') {
            agent {
                label 'Testing Node 1'
            }
            steps {
                echo "Stage 1"
                echo 'Installation and Configuration of Ansible in the slave node: Testing Node 1'
                    sh '''#!/bin/bash
                        yum update -y
                        yum remove ansible -y
                        rpm -q ansible
                        yum install epel-release -y
                        yum install ansible -y 
                        ansible --version
                    '''
            }
        }
        stage('Stage 2') {
            agent {
                label 'Testing-Server'
            }
            steps {
                echo "Stage 2"
                echo 'Installation and Configuration of Ansible in the slave node: Testing-Server'
                    sh '''#!/bin/bash
                        sudo yum update -y
                        yum remove ansible -y
                        rpm -q ansible
                        yum install epel-release -y
                        yum install ansible -y 
                        ansible --version
                    '''
            }
        }    
        stage('Stage 3') {
            agent {
                label 'master'
            }
            steps {
                echo "Stage 3"
                echo 'Installation and Configuration of Ansible in the slave node'
                    sh '''#!/bin/bash
                        sudo ansible all -m ping -u root
                        sudo ansible all -m shell -a "yum install -y git"
                        sudo ansible all -m shell -a "yum install -y chromedriver"
                        sudo ansible all -m shell -a "yum install -y chromium"
                    '''
            }
        }
        stage('Stage 4') {
            agent { docker 'devopsedu/webapp' }
            steps {
                git(
                    url: 'git@github.com:Julesfogue/Project-Certification.git',
                    credentialsId: 'f60dfa64-300b-4108-b181-30fdaeaa4a73',
                    branch: "${branch}"
                )
            }
        }
        stage('Stage 5') {
            steps {                
                sh "mvn clean"
            }
        }
        stage('Stage 6') {
            steps {
                sh "mvn test"
            }
        }
        stage('Stage 7') {
            steps {
                sh "mvn package"
            }
        }
    }
    post {
        success {
            mail to: "jbfogue@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
        }
        failure {
            mail to: "jbfogue@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
        }
    }
}
