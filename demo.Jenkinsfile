pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: 'C:\Program Files (x86)\Jenkins\workspace\pipeline-code\target]\*.war'
                }
            }
        }
    }
}
