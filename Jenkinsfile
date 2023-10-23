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
                    docker run --rm -tdi --name teste simplepythonflask
                    sleep 10
                    docker exec -i teste nosetests --with-xunit --with-coverage --cover-package=project test_users.py
                    docker cp teste:/courseCatalog/nosetests.xml .
                '''
                junit 'nosetests.xml'
            }
        }

        stage('SonarQube'){
            steps{
                script {
                def sonarScannerPath = tool 'SonarScanner'

                sh "${sonarScannerPath}/bin/sonar-scanner \
                    -Dsonar.projectKey=courseCatalog \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://sonarqube:9000 \
                    -Dsonar.login=sqp_9fc2dca8c6d7a070b703767375cb945d83b477e0"
                }
            }
        }
    }

    post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo "Finalizei com sucesso"
            sh 'docker stop teste'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'Ocorreu uma falha'
            sh 'docker stop teste'
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
