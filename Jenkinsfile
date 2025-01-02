pipeline {
    agent any
    tools{
        maven 'sonarmaven'
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token')
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean'
            }
        }

        stage('Run Automation Tests') {
            steps {
                bat 'mvn test verify'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Use a tool like Jacoco to generate the coverage report
                bat 'mvn jacoco:report'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=smth ^
                        -Dsonar.tests=src/test/java ^
                        -Dsonar.java.binaries=target/classes ^
                        -Dsonar.java.test.binaries=target/test-classes ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=%SONAR_TOKEN% ^
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
