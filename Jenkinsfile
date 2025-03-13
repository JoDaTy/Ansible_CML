pipeline {
    agent {
        // Use a specific Jenkins agent that has Ansible and Python installed
        label 'ansible'
    }

    environment {
        ANSIBLE_REPO = 'https://github.com/JoDaTy/Ansible_CML.git'
        GIT_CREDENTIALS_ID = '5cab7cea-4256-43df-bf86-63e9d07b8d54' // Replace with your GitHub credentials ID
        ANSIBLE_VAULT_PASSWORD = credentials('ansible_vault_password') // Ensure this credential is set in Jenkins
    }

    stages {
        stage('Setup Environment') {
            steps {
                script {
                    echo 'Setting up the environment...'
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate'
                    sh 'pip install ansible'
                }
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning Ansible playbooks from GitHub...'
                    git branch: 'main', url: ANSIBLE_REPO
                }
            }
        }

        stage('Add Project') {
            steps {
                script {
                    echo 'Adding new project to Cisco CML using Ansible...'
                    sh '. venv/bin/activate && ansible-playbook -i inventory add_project.yml --vault-password-file=<(echo $ANSIBLE_VAULT_PASSWORD)'
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
