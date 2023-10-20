pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t simplepythonflask .'
            }
        }
        
        stage('Executar Teste') {
            steps {
                sh '''
                    docker run -itd -p 5000:5000 --name teste simplepythonflask
                    sleep 10
                    docker exec -ti teste nosetests --with-xunit --with-coverage --cover-package=project test_users.py
                    docker cp teste:/courseCatalog/nosetests.xml .
                '''
            }
        }
    }

    post {
        // always {
        //     docker stop teste
        // }
        success {
            echo "Finalizado com sucesso"
            sh 'docker stop teste'
        }
        failure {
            echo "Falha no teste"
            sh 'docker stop teste'
        }
    }
}