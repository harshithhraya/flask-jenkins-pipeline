pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh './venv/bin/python -m pip install -r requirements.txt'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh './venv/bin/python -m pytest -v'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'mkdir -p deploy && cp app.py requirements.txt deploy/'
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                    cd deploy
                    ../venv/bin/python -c "
import subprocess, time, urllib.request

p = subprocess.Popen(['../venv/bin/python', 'app.py'])

try:
    for i in range(15):
        try:
            response = urllib.request.urlopen('http://127.0.0.1:5000/', timeout=2)
            body = response.read().decode()
            print('HTTP Status:', response.status)
            print('Response:', body)
            assert response.status == 200
            assert body == 'Flask Application is Running!'
            break
        except Exception:
            time.sleep(1)
    else:
        raise RuntimeError('Application verification failed')
finally:
    p.terminate()
    p.wait()
"
                '''
            }
        }
    }
}