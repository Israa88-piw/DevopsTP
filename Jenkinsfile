pipeline {
    agent any

    tools {
        maven '/usr/local/src/maven'
        jdk '/usr/lib/jvm/java-17-openjdk-amd64'
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
                    def warFile = findFiles(glob: 'target/*.war')[0].path
                    echo "Déploiement du fichier: ${warFile}"
                    
                    // Déployer sur Tomcat
                    deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', 
                                            url: 'http://localhost:8080')], 
                            contextPath: 'devopsapp',
                            war: 'target/*.war'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline CI/CD terminé'
            cleanWs() // Nettoyer l workspace
        }
        success {
            echo 'SUCCÈS: Application déployée sur Tomcat!'
            emailext (
                subject: "SUCCÈS: Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Le build ${env.BUILD_URL} a réussi",
                to: "votre-email@example.com"
            )
        }
        failure {
            echo 'ÉCHEC: Le pipeline a rencontré une erreur'
        }
    }
}
