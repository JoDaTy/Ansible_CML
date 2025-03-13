pipeline {
    agent any

    environment {
        ANSIBLE_REPO = 'https://github.com/JoDaTy/Ansible_CML.git'
        ANSIBLE_BRANCH = 'main'
        ANSIBLE_VAULT_PASSWORD = credentials('ansible_vault_password') // Assuming you have stored your vault password in Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning Ansible playbooks from GitHub...'
                    git branch: ANSIBLE_BRANCH, url: ANSIBLE_REPO
                }
            }
        }

        stage('Add Project') {
            steps {
                script {
                    echo 'Adding new project to Cisco CML using Ansible...'
                    sh 'ansible-playbook -i inventory add_project.yml --vault-password-file=<(echo $ANSIBLE_VAULT_PASSWORD)'
                }
            }
        }

        stage('Configure Router 1') {
            steps {
                script {
                    echo 'Configuring Router 1 using Ansible...'
                    sh 'ansible-playbook -i inventory configure_router1.yml --vault-password-file=<(echo $ANSIBLE_VAULT_PASSWORD)'
                }
            }
        }

        stage('Configure Router 2') {
            steps {
                script {
                    echo 'Configuring Router 2 using Ansible...'
                    sh 'ansible-playbook -i inventory configure_router2.yml --vault-password-file=<(echo $ANSIBLE_VAULT_PASSWORD)'
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
