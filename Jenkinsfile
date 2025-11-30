pipeline {
    agent any

    tools {
        maven '/usr/local/src/maven'
        jdk '/usr/lib/jvm/java-17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/Israa88-piw/DevopsTP.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

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

        stage('Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
            post {
                always {
                    archiveArtifacts 'target/*.war'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Vérifier que le fichier WAR existe
                    def files = findFiles(glob: 'target/*.war')
                    if (files.length > 0) {
                        echo "Fichier WAR trouvé: ${files[0].name}"
                        // Ici vous ajouterez le déploiement plus tard
                    } else {
                        error "Aucun fichier WAR trouvé dans target/"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline CI/CD terminé'
            // Remplacer cleanWs() par deleteDir() qui est disponible
            deleteDir()
        }
        success {
            echo 'SUCCÈS: Build réussi!'
        }
        failure {
            echo 'ÉCHEC: Le pipeline a rencontré une erreur'
        }
    }
}
