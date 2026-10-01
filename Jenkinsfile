pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3.9'
    }

    triggers {
        githubPush()
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    stages {
        stage('Récupération du code') {
            steps {
                git branch: 'main', url: 'https://github.com/votre-compte/votre-repo-backend.git'
            }
        }

        stage('Tests Unitaires') {
            steps {
                echo 'Lancement des tests unitaires...'
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Création du Livrable') {
            steps {
                echo 'Génération du JAR/WAR dans target...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archivage du Livrable') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécutée avec succès ! Livrable disponible dans target/.'
        }
        failure {
            echo 'Échec de la pipeline.'
        }
    }
}
