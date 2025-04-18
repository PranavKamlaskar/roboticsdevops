pipeline {
    agent any

    environment {
        PROJECT_DIR = 'iot_backend'
        VENV_DIR = 'venv'
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/PranavKamlaskar/roboticsdevops.git'
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    source $VENV_DIR/bin/activate
                    pip install -r $PROJECT_DIR/requirements.txt
                '''
            }
        }

        stage('Migrate & Collectstatic') {
            steps {
                sh '''
                    source $VENV_DIR/bin/activate
                    python3 manage.py migrate --settings=iot_backend.settings
                    python3 manage.py collectstatic --noinput --settings=iot_backend.settings
                '''
            }
        }

        stage('Restart Gunicorn') {
            steps {
                sh 'sudo systemctl restart gunicorn'
            }
        }
    }
}

