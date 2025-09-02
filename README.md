pipeline {
    agent any

    parameters {
        booleanParam(name: 'Sonar', defaultValue: true, description: 'Ejecutar análisis de SonarQube')
        booleanParam(name: 'Fortify', defaultValue: false, description: 'Ejecutar análisis de Fortify')
        booleanParam(name: 'BlackDuck', defaultValue: false, description: 'Ejecutar análisis de BlackDuck')
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Configurado en Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Coverage') {
            steps {
                sh '''
                  ./gradlew clean test jacocoTestReport
                '''
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression { params.Sonar }
            }
            steps {
                withSonarQubeEnv('SonarQubeServer') { // Configura este nombre en Jenkins -> System -> SonarQube servers
                    sh '''
                      ./gradlew sonarqube \
                        -Dsonar.projectKey=TU_PROJECT_KEY \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Fortify Scan') {
            when {
                expression { params.Fortify }
            }
            steps {
                echo "Ejecutando Fortify..."
                // Aquí va tu integración de Fortify
            }
        }

        stage('BlackDuck Scan') {
            when {
                expression { params.BlackDuck }
            }
            steps {
                echo "Ejecutando BlackDuck..."
                // Aquí va tu integración de BlackDuck
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/test/TEST-*.xml'
            jacoco execPattern: '**/build/jacoco/test.exec', 
                   classPattern: '**/build/classes/java/main', 
                   sourcePattern: '**/src/main/java', 
                   exclusionPattern: '**/src/test*'
        }
    }
}
