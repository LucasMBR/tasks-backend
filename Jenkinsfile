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
                sh 'mvn clean test verify'
            }
        }
        stage('SOnarAnalysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=1536c99242ec2dfcb48f4bc4ede17c33e9199e3b -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**RootController.java"
                }
            }
        }
    }
}

