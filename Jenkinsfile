pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/zeeumair/fooproject.git'
            }
            
        }
        stage('junit build') {
            steps {
                sh "mvn compile"
            }
        }
        stage('junit test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit '**/TEST*.xml'
                }
            }
        }
      
      stage('newman') {
            steps {
                sh 'newman run Umair_Restful_Booker.postman_collection.json --environment Umair_Restful_Booker.postman_environment.json --reporters junit'
            }
            post {
                always {
                        junit '**/*xml'
                    }
                }
        }
        stage('robot') {
            steps {
                sh 'robot -d results --variable BROWSER:headlesschrome IfotivBooking.robot'
            }
            post {
                always {
                    script {
                          step(
                                [
                                  $class              : 'RobotPublisher',
                                  outputPath          : 'results',
                                  outputFileName      : '**/output.xml',
                                  reportFileName      : '**/report.html',
                                  logFileName         : '**/log.html',
                                  disableArchiveOutput: false,
                                  passThreshold       : 50,
                                  unstableThreshold   : 40,
                                  otherFiles          : "**/*.png,**/*.jpg",
                                ]
                           )
                  
                    }
                }
            }
        }
    }
    post {
         always {
            junit '**/TEST*.xml'
            emailext attachLog: true, attachmentsPattern: '**/TEST*xml',
            body: 'Bod-DAy!', recipientProviders: [culprits()], subject:
            '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!'
         }
    }
}

