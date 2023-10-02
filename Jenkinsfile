pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']], // Je kunt de gewenste branch aanpassen
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline-ubuntu.git']] // Vervang dit door de URL van je GitHub-repo
                    ])
                }
            }
        }
        
        stage('Overwrite HTML files on Test Server') {
            steps {
                sh 'sshpass -p student scp -r /var/lib/jenkins/workspace/Pipeline_test/*.html student@10.10.10.53:/var/www/html/'
            }
        }
        
        stage('Merge to Main') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                script {
                    // Merge de wijzigingen van de 'test' branch naar de 'main' branch
                    sh 'git checkout main'
                    sh 'git merge test'
                    sh 'git push origin main'
                }
            }
        }
    }
