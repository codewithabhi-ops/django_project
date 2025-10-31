pipeline {
    agent any

    environment {
        PROJECT_DIR = "${WORKSPACE}"
        VENV_DIR = "${PROJECT_DIR}/venv"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/codewithabhi-ops/django_project.git'
            }
        }

       stage('Set up Python Environment') {
    steps {
        echo "Creating virtual environment and installing dependencies..."
        sh '''
            cd ${PROJECT_DIR}
            python3 -m venv venv
            bash -c "source venv/bin/activate && pip install -r requirements.txt"
        '''
    }
}


        stage('Collect Static Files') {
    steps {
        echo "Collecting static files..."
        sh '''
            cd ${PROJECT_DIR}
            bash -c "source venv/bin/activate && python manage.py collectstatic --noinput"
        '''
    }
}


       stage('Restart Services') {
    steps {
        echo 'Restarting Gunicorn and NGINX on remote Django server...'
        sh '''
        ssh -o StrictHostKeyChecking=no jenkins@52.63.0.218 "sudo systemctl restart gunicorn && sudo systemctl restart nginx"
        '''
    }
}

    }
}
