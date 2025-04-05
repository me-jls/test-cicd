pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Create RC Tag') {
            when {
                branch 'master'
            }
            steps {
                script {
                    def tagName = 'v1.0.0-rc1' // Exemple, nom du tag à générer
                    def commitSHA = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()

                    // Vérifier que le tag n'existe pas déjà
                    def tagExists = sh(script: "git tag -l ${tagName}", returnStdout: true).trim()

                    if (!tagExists) {
                        // sh "git tag -a ${tagName} -m 'Release Candidate 1 for version 1.0.0' ${commitSHA}"
                        // sh "git push origin ${tagName}"
                        echo "Tag is created"
                    } else {
                        echo "Tag ${tagName} already exists."
                    }
                }
            }
        }
        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to production...'
            // Déployer uniquement à partir de la branche master
            }
        }
    }
}
