pipeline {
    agent none

    environment {
        AUTHOR = 'Ahmad Muharik Al Ansori'
        EMAIL = 'ahmadmuharik@gmail.com'
        WEB = 'https://www.mim.com'
    }

    stages {
        stage('Prepare') {
            environment {
                APP = credentials('harik_desicantik')
            }

            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                echo "Author ${AUTHOR}"
                echo "Email ${EMAIL}"
                echo "Web ${WEB}"
                echo "Start Job : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_DISPLAY_NAME}"
                echo "Branch Name : ${env.BRANCH_NAME}"
                echo "App User : ${APP_USR}"
                echo "App Password : ${APP_PSW}"
            }
        }

        stage('Build') {
            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                script {
                    for (int i = 0; i < 10; i++) {
                        echo "Script ${i}"
                    }
                }
                echo 'Start Build'
                sh 'chmod +x mvnw'
                sh './mvnw clean compile test-compile'
                echo 'Finish Build'
            }
        }

        stage('Test') {
            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                script {
                    def data = [
                        "firstName": "Ahmad",
                        "lastName": "Muharik Al Ansori"
                    ]
                    writeJSON file: 'data.json', json: data
                }
                echo 'Start Test'
                sh 'chmod +x mvnw'
                sh './mvnw test'
                echo 'Finish Test'
            }
        }

         stage('Deploy') {
            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                echo 'Hello Deploy 1'
                sleep(5)
                echo 'Hello Deploy 2'
                echo 'Hello Deploy 3'
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
        }
        success {
            echo 'Yay, success'
        }
        failure {
            echo 'Oh no, failure'
        }
        cleanup {
            echo "Don't care success or error"
        }
    }
}