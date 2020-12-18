pipeline { 
    agent any 
    environment {
        // sets three variables, GITHUb, GITHUB_USR and GITHUB_PSW
        // https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#usernames-and-passwords
        GITHUB = credentials('GITHUB')
    }
    stages {
        stage('Docker Login') { 
            steps { 
                sh 'echo $GITHUB_PSW | docker login ghcr.io -u gimlet-io --password-stdin'
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
        stage('Checkout GitOps repository') {            
            steps {
                sh '''
                    rm -rf gitops || true
                    git config --local credential.username $GITHUB_USR
                    git config --local credential.helper "!echo password=$GITHUB_PSW; echo"
                    git clone https://github.com/gimlet-io/jenkins-example-gitops.git gitops
                    '''
            }
        }
        stage('Fetch Gimlet CLI') {
            steps {
                sh '''
                    curl -L https://github.com/gimlet-io/gimlet-cli/releases/download/tip/gimlet-$(uname)-$(uname -m) -o gimlet
                    chmod +x gimlet
                   '''
            }
        }
        stage('Write to GitOps repository') {
            steps {
                sh '''
                    ./gimlet manifest template \
                      -f .gimlet/staging.yaml |
                        ./gimlet gitops write -f - \
                          --env staging \
                          --app jenkins-example \
                          --gitops-repo-path gitops \
                          -m "Releasing $GIT_COMMIT"
                   '''
            }
        }
        stage('Push to GitOps repository') {
            steps {
                sh '''
                    cd gitops
                    git config --local user.email "jenkins@jenkins.jenkins"
                    git config --local user.name "Jenkins"
                    git push origin main
                   '''
            }
        }
    }
}
