pipeline {
    agent any

    environment {
        GO_BIN = "${WORKSPACE}/go"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url:  'https://github.com/mprashanth424/new1'
            }
        }

        stage('Install Go') {
            steps {
                sh '''
                curl -L -o go.tar.gz https://go.dev/dl/go1.21.0.darwin-amd64.tar.gz
                mkdir -p ${GO_BIN}
                tar -C ${GO_BIN} -xzf go.tar.gz
                export PATH=${GO_BIN}/go/bin:$PATH
                go version
                '''
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh '''
                    export PATH=${GO_BIN}/go/bin:$PATH
                    go mod tidy
                    go build -o workforce-app
                    '''
                }
            }
        }

        stage('Run Backend') {
            steps {
                dir('backend') {
                    sh '''
                    chmod +x workforce-app
                    nohup ./workforce-app --port=8080 &
                    '''
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh '''
                    /opt/homebrew/bin/npm install
                    nohup /opt/homebrew/bin/npm start &
                    '''
                }
            }
        }
    }
}
