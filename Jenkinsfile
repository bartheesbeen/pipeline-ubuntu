pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                checkout([$class: 'GitSCM', branches: [[name: 'test']], userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']]])
            }
        }

        stage('Overwrite HTML files on Test Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server, overwriting existing files.
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Pipeline_test/index.html student@10.10.10.53:/var/www/html/'
            }
        }
        
        stage('Approval') {
            steps {
                input "Wil je doorgaan met het pushen naar de hoofd (main) branch?"
            }
        }

        stage('Push to Main Branch') {
            steps {
                script {
                    def branchName = 'main'
                    def gitCredentials = credentials('f9b6e8ca-d43d-43ae-9939-1798f9273bf7') // Replace with your Git credentials ID
                    checkout([$class: 'GitSCM', branches: [[name: branchName]], userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']]])
                    sh "git config user.name 'bartheesbeen'"
                    sh "git config user.email '509679@student.fontys.nl'"
                    sh "git add ."
                    sh "git commit -m 'Update HTML files'"
                    sh "git push ${gitCredentials}@github.com/bartheesbeen/pipeline-ubuntu.git ${branchName}"
                }
            }
        }
    }
}
