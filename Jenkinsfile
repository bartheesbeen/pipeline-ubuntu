pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']], // You can change the branch as needed
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

        stage('Overwrite HTML files on Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server, overwriting existing files.
                sh 'scp -r /var/lib/jenkins/caches/git-a660bf4669a1e2454fe8754a08ca571a/. student@10.10.10.50:/var/www/html/'
            }
        }
    }
}
