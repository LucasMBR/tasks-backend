pipeline{
    agent any
    stages{
        stage('Build Backend'){
            steps{
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit tests'){
            steps{
                sh 'mvn test'
            }
        }
        stage('SOnarAnalysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=01eecb5eaaec0cc0a0f1dc0762b2ec8f9839978f -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**RootController.java"
                }
            }
        }
    }
}

