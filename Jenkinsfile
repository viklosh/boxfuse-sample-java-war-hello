pipeline {
    agent none

    stages {
        
        stage('buid') {
            agent {
                docker { image 'maven:3-alpine' }
            }
            steps {
                sh 'mvn --version'
            }
        }
    }
}
