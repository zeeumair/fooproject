pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/zeeumair/fooproject.git'
            }
            
        }
      
      stage('newman') {
            steps {
                sh 'newman run Umair_Restful_Booker.postman_collection.json --Umair_Restful_Booker.postman_environment.json --reporters junit'
            }
            post {
                always {
                        junit '**/*xml'
                    }
                }
        }
    }
}

