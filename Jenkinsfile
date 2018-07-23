pipeline {
    agent none
    stages {
        stage("Analyse") {
            agent {
                label "ubuntu"
            }
            tools {
                jdk "zulu-8"
            }
            steps {
                echo "Analysing"
            }
        }
    }
   post {
        always {
            hipchatSend (color: 'YELLOW', credentialId: 'hipchat', textFormat: false, message: "${env.JOB_NAME} build # ${env.BUILD_NUMBER} Completed. See <a href='${BUILD_URL}'>Jenkins</a>")
        }
    } 
}
