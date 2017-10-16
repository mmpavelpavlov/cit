pipeline {
    agent any
    stages {

        stage ("Checkout SCM") {
           steps {
              checkout scm
                script {
                   def branch = env.BRANCH_NAME
                   def commit = sh (script: 'git rev-parse --short HEAD', returnStdout: true).trim()

                   def tag
                        switch (branch) {
                           case 'master':
                                tag = 'master-' + commit
                                break
                           case 'development':
                                tag = 'development-' + commit
                                break
                           default:
                                tag = 'feature-' + commit
                                break
                        }
                   currentBuild.description = tag
              }
           }
        }
        stage('Build') {
            steps {
                echo 'Building..'
		script {
                sh (""" sed -i 's@url = "jdbc:postgresql://localhost/achieve_db?user=postgres\\&password=r00t-pazz"@url = "jdbc:postgresql://raachievedev.cdbfsscmkjwr.us-east-1.rds.amazonaws.com/dev_db?user=RAAchieveDev\\&password=dgaUwwunDY2Pd4jZw58D"@g' file2 """)
		}
		sh "cat file2"
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh "ls -lah"
                sh "cd achieve ; sbt compile"
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Testing...'
                sh "cd achieve ; sbt test"
                sh "ls -lah"
            }
        }
        stage('Deploy') {
                steps {
                        script {
                                if (env.BRANCH_NAME == 'master') {
                                        echo 'I will deploy on the UAT environment'
                                } else if (env.BRANCH_NAME == 'development') {
                                        echo 'I will deploy on the DEV environment'
                                } else { echo 'This is a feature branch, will not be deployed to environment'
                                }
                        }

                }
        }

   }
}

