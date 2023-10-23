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
                    junit 'nosetests.xml'
                '''

                sh "sonar-scanner \
                    -Dsonar.projectKey=curseCatalog \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://192.168.50.222:9000 \
                    -Dsonar.login=sqp_f3cc3b6bc3dcbada6d5c73bc4cf8435961130e42"
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