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
                    sshpass(script: 'ssh -o StrictHostKeyChecking=no student@10.10.10.50 \'cd /var/www/html && rm -f *.html\'', 
                            password: 'student')
                }
            }
        }

        stage('Copy HTML files to Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server.
                script {
                    sh 'scp -r /var/lib/jenkins/caches/git-a660bf4669a1e2454fe8754a08ca571a.html student@10.10.10.50:/var/www/html'
                }
            }
        }
    }
}
