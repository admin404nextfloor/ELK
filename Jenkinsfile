pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = 'site.yml'
        INVENTORY = 'hosts.ini'
        DEPLOY_BRANCH = 'main'
    }

    options {
        timestamps()
        ansiColor('xterm')
    }

    parameters {
        booleanParam(name: 'RUN_DEPLOY', defaultValue: true, description: 'Развернуть стек?')
    }

    triggers {
        cron('H 3 * * *') // Запуск каждый день в 03:00 утра
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${env.DEPLOY_BRANCH}", url: 'https://your-git-repo.example.com/your-project.git'
            }
        }

        stage('Prepare Ansible') {
            steps {
                sh '''
                    sudo apt update
                    sudo apt install -y ansible
                '''
            }
        }

        stage('Deploy Infrastructure') {
            when {
                expression { params.RUN_DEPLOY }
            }
            steps {
                sh '''
                    ansible-playbook -i ${INVENTORY} ${ANSIBLE_PLAYBOOK}
                '''
            }
        }

    }

    post {
        success {
            echo '✅ Развертывание прошло успешно.'
        }
        failure {
            echo '❌ Ошибка развертывания. Проверьте логи.'
        }
    }
}
