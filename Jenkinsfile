properties([
    parameters([
        string(defaultValue: 'site', name: 'Playbook Name'),
        choice(choices: ['Dry-Run','Playbook-deploy'], name: 'Playbook Action')
    ])
])
pipeline {
    agent any 
    stages {
        stage('Preparing') {
            steps{
                sh 'echo Preparing'
            }
        }
        stage('Git Pulling') {
            steps{
                git branch: 'master', url: 'https://github.com/imrankazi-cloud/CICD-Ansible.git'
            }
        }
        stage('Playbook Initializing') {
            steps{
                sh 'echo Playbook Initializing'
            }
        }
        stage('Playbook Running') {
    when {
        expression { params['Playbook Action'] == 'Dry-Run' || params['Playbook Action'] == 'Playbook-deploy' }
    }
    steps {
        script {
            if (params['Playbook Action'] == 'Dry-Run') {
                withCredentials([
                    sshUserPrivateKey(credentialsId: 'ansible-connect',
                                      keyFileVariable: 'ANSIBLE_KEY',
                                      usernameVariable: 'ANSIBLE_USER')
                ]) {
                    sh 'ansible-playbook --check -i /etc/ansible/hosts --user "$ANSIBLE_USER" --private-key "$ANSIBLE_KEY" --host-key-checking=no ${params['Playbook Name']}.yml'
                    }
                                          
            } else if (params['Playbook Action'] == 'Playbook-deploy') {
                ansiblePlaybook(
                    credentialsId: 'ansible-connect',
                    disableHostKeyChecking: true,
                    inventory: '/etc/ansible/hosts',
                    playbook: "${params['Playbook Name']}.yml"
                )
            }
        }
    }
}
        stage('Playbook deployed') {
            steps{
                sh 'echo Deployment done!!!!'
            }
        }
    }
}
