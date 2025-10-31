pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning the repository...'
                checkout scm
            }
        }

        stage('Set up Python Environment') {
            steps {
                echo 'Setting up virtual environment...'
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running Django tests...'
                sh '''
                source venv/bin/activate
                python manage.py test
                '''
            }
        }

        stage('Collect Static Files') {
            steps {
                echo 'Collecting static files...'
                sh '''
                source venv/bin/activate
                python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Restart Gunicorn') {
            steps {
                echo 'Restarting Gunicorn service...'
                sh 'sudo systemctl restart gunicorn'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
