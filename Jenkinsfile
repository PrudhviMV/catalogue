pipeline{
    // This is Pre-build section
    agent{
        node {
            label 'Agent-1'
        }
    }
    
    environment{
        Learn = "Jenkins"
        appVersion = ""
    }

    options{
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }


    // This is build section
    stages{
        stage('Read Version'){
            steps{
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "appversion: ${appVersion}"
                }
            }
        }
    
    // This is test section
        stage('Test Dependencies'){
            steps{
                script{
                    sh """
                    npm test
                    """
                }
            }
        }

        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                    npm install
                    """
                }
            }
        }

        stage('Build docker image'){
            steps{
                scripts{
                    sh """
                    docker build -t catalogue:${appVersion} .
                    docker images
                    """
                }
            }
        }

    // This is Deploy section
        stage('Deploy'){          
            steps{
                script{
                    sh """
                    echo 'Deploying'
                    echo '$Learn'
                    """
                }
            }
        }
    }

    post{
        always{
            echo 'I will run always'
            cleanWs()
        }

        aborted{
            echo "Pipeline is aborted for "
        }
        success{
            echo 'I will run if it is success'
        }

        failure{
            echo 'I will run if it is failure $BUILD_URL and $BUILD_TAG'
        }
    }
}
