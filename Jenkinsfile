// Jenkinsfile to add a new project with two routers to Cisco CMP using Ansible from GitHub

pipeline {
    agent any

    environment {
        ANSIBLE_REPO = 'https://github.com/JoDaTy/Ansible_CML.git'
        ANSIBLE_BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo 'Cloning Ansible playbooks from GitHub...'
                    // Clone the repository containing the Ansible playbooks
                    git branch: ANSIBLE_BRANCH, url: ANSIBLE_REPO
                }
            }
        }

        stage('Add Project') {
            steps {
                script {
                    echo 'Adding new project to Cisco CMP using Ansible...'
                    // Execute the Ansible playbook for adding a project
                    sh 'ansible-playbook -i inventory add_project.yml'
                }
            }
        }

        stage('Configure Router 1') {
            steps {
                script {
                    echo 'Configuring Router 1 using Ansible...'
                    // Execute the Ansible playbook for configuring the first router
                    sh 'ansible-playbook -i inventory configure_router1.yml'
                }
            }
        }

        stage('Configure Router 2') {
            steps {
                script {
                    echo 'Configuring Router 2 using Ansible...'
                    // Execute the Ansible playbook for configuring the second router
                    sh 'ansible-playbook -i inventory configure_router2.yml'
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
