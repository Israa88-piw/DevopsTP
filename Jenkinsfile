pipeline {
    agent any

    tools {
        maven '/usr/local/src/maven'  // Utilisez le nom EXACT de Jenkins
        jdk '/usr/lib/jvm/java-17-openjdk-amd64'  // Utilisez le nom EXACT de Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
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
        }

        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
