pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
             sh  "mvn clean"
            }
        }

        stage('Test') {
            steps {
             sh  "mvn test"
            }
        }

        stage('Package') {
            steps {
             sh  "mvn package"
            }
            post {
                always {
                    junit 'tarjet/surefire-reports/*.xml'
                }
                success {
                    archiveArtifacts artifacts: 'tarjet/*.jar', followSymlinks: false
                }
            }
        }

        stage('Deploy') {
            steps {
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']) {
                    sh  "nohub java -jar -Dserver.port=8001 tarjet/*.jar &"
                }
            }
        }
    }
}
