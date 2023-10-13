pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN_HOME"
    }

    stages {
        stage('Clone') {
            steps {
                timeout(time: 4, unit: 'MINUTES'){
                    git branch: 'master', credentialsId: 'github_pat_11AXZA7LY0IjPsqCYMhmqv_pnJTCw2xXxfYebV5ZAuieai8Ww6UMpbEbZqojlw3dWe77S5KE4CPDXOjEV', url: 'https://github.com/FransPaxiJ/sonar-prueba.git'
                }
            }
        }
        stage('Build') {
            steps {
                timeout(time: 20, unit: 'MINUTES'){
                    sh "mvn -DskipTests clean package -f SysAsistenciaAn/pom.xml"
                }
            }
        }
        stage('Test') {
            steps {
                timeout(time: 20, unit: 'MINUTES'){
                    // Se cambia <test> por <install> para que se genere el reporte de jacoco
                    sh "mvn clean install -f SysAsistenciaAn/pom.xml"
                }
            }
        }
        stage('Sonar') {
            steps {
                timeout(time: 20, unit: 'MINUTES'){
                    withSonarQubeEnv('sonarqube'){
                        sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar -Pcoverage -f SysAsistenciaAn/pom.xml"
                    }
                }
            }
        }
        stage('Quality gate') {
            steps {

                sleep(10) //seconds

                timeout(time: 20, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "mvn spring-boot:run -f SysAsistenciaAn/pom.xml"
            }
        }
    }
}