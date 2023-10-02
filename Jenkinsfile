pipeline {
    agent any

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'test']], userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']]])
            }
        }

        stage('Set Git Credentials') {
            steps {
                script {
                    sh 'git config --global user.name "bartheesbeen"'
                    sh 'git config --global user.email "509679@student.fontys.nl"'
                }
            }
        }

        stage('Checkout from GitHub') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'test']], userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']]])
            }
        }

        stage('Overwrite HTML files on Test Server') {
            steps {
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
                    
                    sh "git checkout ${mainBranchName}"
                    sh "git merge ${testBranchName} -m 'Merge test branch into main'"
                    sh "git push origin ${mainBranchName}"
                }
            }
        }
    }
}
