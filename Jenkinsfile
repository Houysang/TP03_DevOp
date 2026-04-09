pipeline {
    agent {
        label 'laravel'
    }

    tools {
        git 'Default'
    }

    stages {
        stage('Build Frontend') {
            steps {
                sh '''
                export NODE_OPTIONS=--max_old_space_size=2048
                npm install
                npm run build
                '''
            }
        }

        stage('Build Frontend (Fix Memory Issue)') {
            steps {
                sh '''
                export NODE_OPTIONS=--max_old_space_size=2048
                npm run build
                '''
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Houysang/TP03_DevOp.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                checkout scm

                echo 'Copy env file...'
                sh 'cp .env.example .env'

                //config database
                echo 'Configuring database...'
                // sh 'sed -i "s/DB_HOST=.*/DB_HOST=localhost/" .env'
                // sh 'sed -i "s/DB_PORT=.*/DB_PORT=3306/" .env'
                // sh 'sed -i "s/DB_DATABASE=*/DB_DATABASE=laravel/" .env'
                // sh 'sed -i "s/DB_USERNAME=*/DB_USERNAME=root/" .env'

                echo 'Installing dependencies...'
                sh 'composer install'
                sh 'npm install'

                echo 'Generating application key...'
                sh 'php artisan key:generate'

                echo 'Installing PHP dependencies...'
                sh 'composer install'

                echo 'Installing Node dependencies...'
                sh 'npm install'

                echo 'Building frontend assets...'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'php artisan test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sh 'ansible-playbook -i inventory/hosts.ini deploy.yml'

            }
        }
    }
}