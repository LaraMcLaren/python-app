pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/LaraMcLaren/python-app.git'
            }
        }

        stage('Deploy to App Server') {
            steps {
                sh '''
                ssh ec2-user@13.234.17.143 << EOF
                    rm -rf /var/www/app/*
                    exit
                EOF

                scp -r * ec2-user@13.234.17.143:/var/www/app/
                '''
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                ssh ec2-user@13.234.17.143 << EOF
                    cd /var/www/app
                    pip3 install -r requirements.txt
                    pkill gunicorn || true
                    nohup gunicorn -b 0.0.0.0:5000 app:app &
                EOF
                '''
            }
        }
    }

}
