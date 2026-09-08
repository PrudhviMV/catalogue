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
        Account_ID = "764038423244"
        project = "roboshop"
        Component = "catalogue"
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
        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                    npm install
                    """
                }
            }
        }

        stage('Test Dependencies'){
            steps{
                script{
                    sh """
                    npm test
                    """
                }
            }
        }

        stage('Build docker image'){
            steps{
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${Component}:${appVersion} .
                        docker push ${Account_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${Component}:${appVersion}
                    """
                    }
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
