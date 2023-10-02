pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']], // Je kunt de gewenste branch aanpassen
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']] // Vervang door de URL van je GitHub-repo
                    ])
                }
            }
        }

        stage('Delete HTML files on Server') {
            steps {
                // Execute shell commands to delete HTML files on the server.
                script {
                    sshagent(['abb4f129-cb4f-496c-86d0-01a08e343ff5']) {
                        sh 'ssh -o StrictHostKeyChecking=no student@10.10.10.50 \'cd /var/www/html && rm -f *.html\''
                    }
                }
            }
        }

        stage('Copy HTML files to Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server.
                script {
                    sshagent(['your-ssh-credentials-id']) {
                        sh 'scp -r /var/lib/jenkins/caches/git-a660bf4669a1e2454fe8754a08ca571a.html student@10.10.10.50:/var/www/html'
                    }
                }
            }
        }
    }
}
