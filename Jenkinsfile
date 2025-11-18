pipeline {
    agent any
    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }
    stages {
        stage('GIT') {
            steps {
                echo "Checkout handled by Jenkins"
            }
        }
        stage('Compile Stage') {
            steps {
                sh 'mvn -v'
                sh 'mvn clean compile'
            }
        }
    }
}
