// Jenkinsfile
pipeline {
    agent any

    tools {
        // Most már a helyes névvel hivatkozunk a Maven telepítésre!
        maven 'maven-3.9.11'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Forráskód lekérése Git-ből...'
            }
        }

        stage('Build') {
            steps {
                echo 'Az alkalmazás fordítása Maven-nel...'
                withMaven {
                    bat 'mvn clean compile'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Unit tesztek futtatása Maven-nel...'
                withMaven {
                    bat 'mvn test'
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Az alkalmazás becsomagolása JAR fájlba...'
                withMaven {
                    bat 'mvn package'
                }
            }
        }

        stage('Run Application') {
            steps {
                echo 'A becsomagolt alkalmazás futtatása...'
                bat 'java -cp target/hello-1.0-SNAPSHOT.jar learningjenkins.App'
            }
        }
    }

    post {
        always {
            echo 'Pipeline futás befejeződött.'
        }
        success {
            echo 'Pipeline sikeresen lefutott! 🎉'
        }
        failure {
            echo 'Pipeline hibával leállt! 😢'
        }
    }
}
