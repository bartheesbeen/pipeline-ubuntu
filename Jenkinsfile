pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']], // You can change the branch as needed
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']] // Replace with your GitHub repo URL
                    ])
                }
            }
        }
        
        stage('Overwrite HTML files on Test Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server, overwriting existing files.
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Pipeline_test/index.html student@10.10.10.53:/var/www/html/'
            }
        }
    }
    post {
        always {
            script {
                def userInput = input(
                    id: 'deployToTest',
                    message: 'Wil je doorgaan met het deployen naar de test server?',
                    parameters: [boolean(defaultValue: true, description: 'Doorgaan?', name: 'DeployToTest')]
                )

                if (userInput.DeployToTest) {
                    // Push naar de 'test'-branch in de externe repository
                    sh 'git push origin test'
                } else {
                    currentBuild.result = 'ABORTED'
                    error('Deployment naar test geannuleerd')
                }
            }
        }
    }
}
