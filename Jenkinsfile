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
        stage('Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=1536c99242ec2dfcb48f4bc4ede17c33e9199e3b -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**RootController.java"
                }
            }
        }
        stage('Quality Gate'){
            steps{
                sleep(5)
                timeout(time:1, unit: 'MINUTES'){
                waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy Backend'){
            steps{
                git 'https://github.com/LucasMBR/tasks-backend'
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('API Test'){
            steps{
                git 'https://github.com/LucasMBR/tasks-api-test'
                sh 'mvn clean test verify'
            }
        }
        stage('Deploy Frontend'){
            steps{
                dir('frontend'){
                git 'https://github.com/LucasMBR/tasks-frontend'
                sh 'mvn clean package'
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage('Functional Test'){
            steps{
                git 'https://github.com/LucasMBR/tasks-functional-tests'
                sh 'mvn clean package'
                
            }
        }
        stage('Deploy Prod'){
            steps{
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
    }
}

