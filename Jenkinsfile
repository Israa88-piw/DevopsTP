pipeline {
    agent any

    tools {
        maven 'maven-3.9.11'  // Doit correspondre au nom configuré dans Jenkins
        jdk 'jdk-17'       // Doit correspondre au nom configuré dans Jenkins
    }

    stages {
        // Étape 1: Récupération du code
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/Israa88-piw/DevopsTP.git',
                credentialsId: 'github-credentials'  // Si besoin d'authentification
            }
        }

        // Étape 2: Construction
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        // Étape 3: Tests unitaires
        stage('Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        // Étape 4: Packaging
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        // Étape 5: Déploiement sur Tomcat
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9('tomcat-server')], 
                contextPath: 'monapp',
                war: 'target/*.war'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé - vérifiez les résultats du build'
        }
        success {
            echo 'SUCCÈS: Application déployée avec succès!'
        }
        failure {
            echo 'ÉCHEC: Le pipeline a rencontré une erreur'
        }
    }
}
