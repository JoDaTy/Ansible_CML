pipeline {
    agent any

    environment {
        ANSIBLE_REPO = 'https://github.com/JoDaTy/Ansible_CML.git'
        ANSIBLE_BRANCH = 'main'
        CML_HOST = '192.168.1.198'
    }

    stages {
        stage('Setup Environment') {
            steps {
                script {
                    echo 'Setting up the environment...'
                    sh 'python3 -m venv /home/jenkins/agent/workspace/CML_Build/'
                    sh '. /home/jenkins/agent/workspace/CML_Build/bin/activate'
                    sh 'chown -R jenkins venv'
                    echo '###############################'
                    sh 'whoami'
                    sh 'pwd'
                    sh 'ls -al'
                    echo '###############################'
                }
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning Ansible playbooks from GitHub...'
                    git branch: ANSIBLE_BRANCH, url: ANSIBLE_REPO
                }
            }
        }

        stage('Run Ansible') {
                steps {
                    ansiblePlaybook colorized: true, credentialsId: 'CML_Creds', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.ini', playbook: '/home/jenkins/agent/workspace/CML_Build/', vaultTmpPath: ''
            }
        }

        stage('Tidy up') {
            steps {
                script {
                    echo 'Deactivte Python virtual environment'
                    sh 'deactivate'
                }
            }
        }
    }

    post {
        success {
            echo 'Project and routers configured successfully.'
        }
        failure {
            echo 'Failed to configure project and routers.'
        }
    }
}
