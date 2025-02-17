pipeline {
    agent any
    
    environment {
        PRISMA_API_URL="${PRISMA_API_URL}"
    }
    
    stages {
        stage('Checkout') {
          steps {
              git branch: 'main', url: "${REPO}"
              stash includes: '**/*', name: 'source'
          }
        }
        stage('PrismaCloud Scanning') {
            steps {
                    script {
                        docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                          try {
                            sh '''
                                checkov --version
                                checkov -d . -o cli -o junitxml --output-file-path console,results.xml --bc-api-key ${PRISMA_API_KEY} --compact --quiet --soft-fail
                            '''
                          } catch (err) {
                              throw err
                        }
                    }
                }
            }
        }
    }
}
