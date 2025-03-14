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
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate && pip install ansible'
                    sh 'cd venv'
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

        stage('Run Ansible Playbooks') {
            environment {
                ANSIBLE_HOST_KEY_CHECKING = 'False'
            }
            steps {
                script {
                    echo 'Executing Ansible playbooks...'
                    withCredentials([
                        [$class:'UsernamePasswordMultiBinding', credentialsId: 'cml_credentials', usernameVariable: 'ANSIBLE_USER', passwordVariable: 'ANSIBLE_PASS']
                        ]) {
                        sh '. venv/bin/activate && ansible-playbook -i inventory.ini add_project.yml -u $ANSIBLE_USER --extra-vars password=$ANSIBLE_PASSWORD'
                        sh 'pwd'
                        sh 'ls -al'
                    }
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
