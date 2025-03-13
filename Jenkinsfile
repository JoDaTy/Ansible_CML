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
                        [$class:'UsernamePasswordMultiBinding', credentialsId: 'cml_username', usernameVariable: 'ANSIBLE_USER', passwordVariable: 'ANSIBLE_PASS']
                        ]) {
                        sh '. venv/bin/activate && ansible-playbook -i inventory add_project.yml --user $ANSIBLE_USER --password $ANSIBLE_PASSWORD'
                        sh '. venv/bin/activate && ansible-playbook -i inventory configure_router1.yml --user $ANSIBLE_USER --password $ANSIBLE_PASSWORD'
                        sh '. venv/bin/activate && ansible-playbook -i inventory configure_router2.yml --user $ANSIBLE_USER --password $ANSIBLE_PASSWORD'
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
