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
                checkoutAndBuildSource()
            }
        }
    }
   post {
        always {
            hipchatSend (color: 'YELLOW', credentialId: 'hipchat', textFormat: false, message: "${env.JOB_NAME} build # ${env.BUILD_NUMBER} Completed. See <a href='${BUILD_URL}'>Jenkins</a>")
        }
    } 
}

def checkoutAndBuildSource(){
    echo 'JAVA_HOME = ' + JAVA_HOME
    prNo = env.BRANCH_NAME
    script{
        dir('src'){
            deleteDir()
        }
    }
    checkout changelog: false, 
      poll: false, 
      scm: [$class: 'GitSCM', 
      branches: [[name: "*/master"]], 
      doGenerateSubmoduleConfigurations: false, 
      extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'src']], 
    submoduleCfg: [], 
    userRemoteConfigs: [[url: 'https://github.com/payara/Payara.git']]]
    withCredentials([[$class: 'StringBinding', credentialsId: '29c66d4a-6529-42cc-b5a9-c6887be62d49', variable: 'githubToken']]) {
        dir('src') {
            sh """mvn clean package \
            -DskipTests \
            -Dsonar.organization=payara \
            -Dsonar.host.url=https://sonarcloud.io \
            -Dsonar.pullrequest.provider=github \
            -Dsonar.analysis.mode=preview \
            -Dsonar.pullrequest.github.repository=MarkWareham/Payara \
            -Dsonar.pullrequest.github.endpoint=https://api.github.com/ \
            -Dsonar.pullrequest.branch=${env.BRANCH_NAME} \
            -Dsonar.pullrequest.key=${prNo} \
            -Dsonar.pullrequest.base=master \
            -Dsonar.github.oauth=${githubToken} \
            -Dsonar.login=3853c5117da15393e564a186ff155dcb44ef8c9e \
            sonar:sonar"""
        }
    }
}