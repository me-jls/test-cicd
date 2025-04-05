pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -ltr'
            }
        }

        stage('Check File and Setup Parameters') {
            steps {
                script {
                    def fileExists = fileExists('README.md')
                    if (fileExists) {
                        properties([
                            parameters([
                                string(defaultValue: 'yesss', description: 'Enter value since file exists', name: 'CONDITIONAL_PARAM')
                            ])
                        ])
                        echo 'Conditional parameter added.'
                    } else {
                        echo 'File not found. Skipping parameter setup.'
                    }
                }
            }
        }

        stage('Read YAML') {
            steps {
                script {
                    // Charger la fonction de lecture YAML
                    yamlContent = readYaml(file: 'config-dev.yml')
                    // Exemple d'utilisation des données lues
                    echo "Version: ${yamlContent.versions.api.version}"

                // Vous pouvez maintenant utiliser `yamlContent` dans les prochains stages
                }
            }
                }

        stage('Use Parameter') {
            when {
                expression { params.CONDITIONAL_PARAM }
            }
            steps {
                script {
                    echo "Using conditional parameter: ${params.CONDITIONAL_PARAM}"
                    yamlContent.alarms.each { dataAlarm ->
                        echo "Version: ${dataAlarm.name}"
                    }
                }
            }
        }

        stage('Create RC Tag') {
            when {
                branch 'master'
            }
            steps {
                script {
                    def tagName = 'v1.0.0-rc1' // Exemple, nom du tag à générer
                    //def commitSHA = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()

                    // Vérifier que le tag n'existe pas déjà
                    def tagExists = sh(script: "git tag -l ${tagName}", returnStdout: true).trim()

                    if (!tagExists) {
                        // sh "git tag -a ${tagName} -m 'Release Candidate 1 for version 1.0.0' ${commitSHA}"
                        // sh "git push origin ${tagName}"
                        echo 'Tag is created'
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
