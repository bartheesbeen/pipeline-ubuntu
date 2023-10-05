pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Code checken in Github.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']], // You can change the branch as needed
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

        stage('User Input') {
            steps {
                input "Wil je doorgaan met het deployen naar de productie server?"
            }
        }

        stage('Overwrite HTML files on Server') {
            steps {
                // Overwrite bestaande HTML files.
                sh 'sshpass -p student scp -r /var/lib/jenkins/workspace/Pipeline_main/*.html student@10.10.10.50:/var/www/html/'
            }
        }
    }
}
