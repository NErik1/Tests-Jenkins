// Jenkinsfile
pipeline {
    // Meghatározza, hogy hol fusson a Pipeline.
    // 'any' azt jelenti, hogy bármelyik elérhető Jenkins agenten futhat.
    // Ha specifikus agentet szeretnél, pl. Windows-t, akkor így add meg:
    // agent { label 'windows-agent' } // Feltételezve, hogy van ilyen labelű agented
    agent any

    // A Pipeline szakaszai
    stages {
        stage('Checkout') {
            steps {
                // Ez a lépés általában implicit, ha a Pipeline SCM-ből (pl. Git) indul.
                // A Jenkins automatikusan letölti a kódot.
                // Ha speciális Git beállításokra van szükséged, itt adhatod meg.
                echo 'Forráskód lekérése Git-ből...'
            }
        }

        stage('Build') {
            steps {
                echo 'Az alkalmazás fordítása Maven-nel...'
                // Mivel Windows agenten futottál korábban, a 'bat' parancsot használjuk.
                // Linux/Unix esetén 'sh' lenne.
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Unit tesztek futtatása Maven-nel...'
                bat 'mvn test'
            }
            // A 'post' szekció a szakasz után fut le, függetlenül attól, hogy sikeres volt-e.
            post {
                always {
                    // A JUnit teszteredmények publikálása a Jenkins UI-ban.
                    // Ez generálja a tesztjelentéseket és grafikonokat.
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Az alkalmazás becsomagolása JAR fájlba...'
                bat 'mvn package'
            }
        }

        stage('Run Application') {
            steps {
                echo 'A becsomagolt alkalmazás futtatása...'
                // Ez futtatja a Maven által generált JAR fájlt.
                // A "Hello World!" kimenet itt fog megjelenni a Jenkins konzolján.
                bat 'java -cp target/hello-1.0-SNAPSHOT.jar learningjenkins.App'
            }
        }
    }

    // A 'post' szekció a teljes Pipeline futása után fut le.
    post {
        always {
            echo 'Pipeline futás befejeződött.'
        }
        success {
            echo 'Pipeline sikeresen lefutott! 🎉'
        }
        failure {
            echo 'Pipeline hibával leállt! 😢'
            // Itt lehetne értesítést küldeni, pl. emailt.
        }
        // cleanWs {
        //     // Ez a lépés törli a workspace-t a futás után.
        //     // Hasznos lehet, ha tiszta környezetet szeretnél minden build előtt.
        // }
    }
}
