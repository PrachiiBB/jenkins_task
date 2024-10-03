pipeline {
    agent any

    stages {
        stage('Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/PrachiiBB/jenkins_task.git'
            }
        }

        stage('Stop Flask App') {
            steps {
                sh '''
                    sudo ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/mynewkey root@192.168.0.108 '
                    FLASK_PID=$(sudo ps -ef | grep myapp.py | grep -v grep | awk "{print \$2}");
                    echo "FLASK_PID: $FLASK_PID";  # Debug statement
                    if [ ! -z "$FLASK_PID" ]; then
                        sudo kill -9 $FLASK_PID;
                        echo "Stopped running Flask app with PID: $FLASK_PID";
                    else
                        echo "No running Flask app found";
                    fi'
                '''
            }
        }

        stage('Transfer') {
            steps {
                sh 'sudo scp -o StrictHostKeyChecking=no -i /var/lib/jenkins/mynewkey /var/lib/jenkins/workspace/PIPELINEAPP6/myapp.py root@192.168.0.108:/root'
            }
        }

        stage('Start Flask App') {
            steps {
                sh '''
                    sudo ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/mynewkey root@192.168.0.108 '
                    cd /root;
                    sudo nohup python3 myapp.py > flask.log 2>&1 &'
                '''
            }
        }
    }
}
