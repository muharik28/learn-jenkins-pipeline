pipeline {
    agent none

    environment {
        AUTHOR = 'Ahmad Muharik Al Ansori'
        EMAIL = 'ahmadmuharik@gmail.com'
        WEB = 'https://www.mim.com'
    }

    // triggers {
    //     cron('*/5 * * * *')
    //     // pollSCM('*/5 * * * *')
    //     // upstream(upstreamProjects: 'belajar-pipeline,Belajar Jenkins', threshold: hudson.model.Result.SUCCESS)
    // }

    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "What is your name?")
        text(name: "DESCRIPTION", defaultValue: "", description: "Tell me about you")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to Deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ['Instagram', 'Facebook', 'Twitter'], description: "Which Sosial Media?")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 20, unit: 'MINUTES')
    }

    stages {
        stage('Parameter') {
            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                echo "Hello ${params.NAME}"
                echo "You description is ${params.DESCRIPTION}"
                echo "Your social media is ${params.SOCIAL_MEDIA}"
                echo "Need to deploy : ${params.DEPLOY} to deploy!"
                echo "Your secret is ${params.SECRET}"
            }
        }

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
                sh 'echo "App Password : ${APP_PSW}" > "desicantik.txt"'
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
            input {
                message 'Can we deploy?'
                ok 'Yes, of course'
                submitter 'superadmin,muharik'
                parameters {
                    choice(name: 'TARGET_ENV', choices: ['DEV', 'QA', 'PROD'], description: 'Which Enviroment?')
                }
            }

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

        stage('Release') {
            when {
                expression {
                    return params.DEPLOY
                }
            }

            agent {
                node {
                    label 'linux'
                }
            }

            steps {
                echo 'Release it'
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