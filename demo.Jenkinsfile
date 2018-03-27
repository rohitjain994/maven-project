pipeline {
    agent any
    tool {
        maven : 'LocalMaven'
    }
    stages{
        stage('Build'){
            steps {
                bat "mvn clean package"
            }
            post {
                success {
                   bat '''
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**\\target\\*.war'
                    '''
                }
            }
        }
    }
}
