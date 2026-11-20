pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Unit Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'start /B python app.py'
            }
        }

        stage('Verify Application') {
            steps {
                bat 'python -c "import urllib.request; r=urllib.request.urlopen(\'http://127.0.0.1:5000/\'); print(r.status); print(r.read().decode()); assert r.status == 200"'
            }
        }
    }
}