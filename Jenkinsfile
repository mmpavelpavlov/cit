pipeline {
    agent any

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
            //    sh "cd achieve ; sbt compile"
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
		steps {
			script {
				if (env.BRANCH_NAME == 'master') {
					echo 'I will build on the UAT environment'
				} else if (env.BRANCH_NAME == 'development') {
					echo 'I will build on the DEV environment'
				} else { echo 'This is a feature branch, will not be deployed to environment'
				}	
			}		

		}
        }

    }
}
