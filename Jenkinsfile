pipeline {
    agent any

    stages {

        // Étape 1 : récupérer le projet depuis le dépôt Git
        stage('Checkout') {
            steps {
                checkout scm
                echo "Commit en cours de compilation : ${env.GIT_COMMIT}"
            }
        }

        // Étape 2 : compiler le projet Spring Boot avec Maven
        stage('Compile') {
            steps {
                script {
                    // Le projet Spring Boot peut être :
                    //  - à la racine du dépôt (dépôt ne contenant que le backend)
                    //  - dans le sous-dossier backend/ (dépôt contenant backend + frontend)
                    def projetDir = fileExists('pom.xml') ? '.' : 'backend'

                    dir(projetDir) {
                        echo "Compilation Maven dans le dossier : ${projetDir}"

                        if (isUnix()) {
                            sh 'mvn compile'
                        } else {
                            bat 'mvn compile'
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'SUCCESS : le projet Spring Boot a été compilé avec Maven.'
        }
        failure {
            echo 'FAILURE : consulter la Console Output pour identifier l\'erreur.'
        }
    }
}
