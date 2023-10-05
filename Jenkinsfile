pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Code in GIthub checken
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']], 
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']]
                    ])
                }
            }
        }

        stage('Overwrite HTML files on Test Server') {
            steps {
                // Overwrite bestaande files in de HTML bestandslocatie.
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Pipeline_test/index.html student@10.10.10.53:/var/www/html/'
            }
        }
    }
}
