pipeline {
    agent any

    stages {

        stage ("Checkout SCM") {
           steps {
              checkout scm
           }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh "ls -lah"
                sh "cd achieve ; sbt compile"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
        //      sh "cd achieve ; sbt test"
                sh "ls -lah"
            }
        }
        stage('Deploy') {
           when { expression { return env.BRANCH_NAME != 'development'; } }
              steps { echo 'This is not the dev branch, will not deploy ....'}
        }
    }
}
node {
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
