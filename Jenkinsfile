pipeline {
    agent any

    environment {
        DEPLOY_USER = 'gon' // User on frontVM
        DEPLOY_HOST = '172.31.253.98' // IP of frontVM
        DEPLOY_PATH = '/var/www/react-app' // Path on frontVM
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', // Your branch
                    credentialsId: '468aceec-2418-40f6-939e-9cd09038f26a',
                    url: 'https://github.com/ayoubMah/reactDemo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'DeploymentFront', // Replace with your SSH server config name
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'dist/**/*', // Where your build artifacts are
                                    removePrefix: 'dist/',
                                    remoteDirectory: "${DEPLOY_PATH}",
                                    execCommand: '' // No restart command needed for static files
                                )
                            ],
                            usePty: false,
                            continueOnError: false,
                            failOnError: true
                        )
                    ]
                )
            }
        }
    }
      triggers {
        githubPush()
    }
}