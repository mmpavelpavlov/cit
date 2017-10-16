pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
		script {
sh """
if netstat -plantu | grep -i :9092; then
    echo kafka is running
else
    echo kafka is not running; exit 1
fi
if netstat -plantu | grep -i :2181; then
    echo ZooKeeper is running
else
    echo ZooKeeper is not running; exit 1
fi
if netstat -plantu | grep -i :11211; then
    echo Memcached is running
else
    echo Memcached is not running; exit 1
fi
"""		

                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
