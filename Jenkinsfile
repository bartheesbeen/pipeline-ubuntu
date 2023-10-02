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
                    def testBranchName = 'test'
                    def mainBranchName = 'main'
                    
                    // Controleer of Jenkins zich in de juiste branch (test) bevindt
                    def currentBranch = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    if (currentBranch != testBranchName) {
                        error('Jenkins is niet in de juiste branch (test).')
                    }
                    
                    // Voer de commit- en push-stappen uit naar de main branch
                    sh "git config user.name 'bartheesbeen'"
                    sh "git config user.email '509679@student.fontys.nl'"
                    sh "git checkout ${mainBranchName}"
                    sh "git merge ${testBranchName} -m 'Merge test branch into main'"
                    sh "git push origin ${mainBranchName}"
                }
            }
        }
    }
}
