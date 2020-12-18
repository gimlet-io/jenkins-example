pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    environment {
        GITHUB_PAT = credentials('GITHUB_PAT')
    }
    stages {
        stage('Docker Login') { 
            steps { 
                sh 'echo $GITHUB_PAT | docker login ghcr.io -u gimlet-io --password-stdin'
            }
        }
        stage('Docker Build'){
            steps {
                sh 'docker build -t ghcr.io/gimlet-io/jenkins-example:$GIT_COMMIT .'
            }
        }
        stage('Docker Push') {
            steps {
                sh 'docker push ghcr.io/gimlet-io/jenkins-example:$GIT_COMMIT'
            }
        }
    }
}
