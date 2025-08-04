pipeline{
    agent any
    tools{
        jdk "JDK 17"
        maven "MAVEN3.9"
    }
     stages{
        stage('Fetch Code from git'){
            steps{
                git branch: 'develop', url: 'https://github.com/grandeefidel/spark_java_app_ci_cd'
            }
        }

        stage('Build Stage'){
            steps{
                sh 'mvn install -DskipTests'
            }
            post{
                success{
                    echo "Archiving artifact"
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }

        stage('Run Unit Test'){
            steps{
                sh 'mvn test'
            }
        }

        stage('checkstyle analysis'){
            steps{
                sh  'mvn checkstyle:checkstyle'
            }
        }


          stage("SonarQube analysis") {
            environment{
                scannerHome = tool 'sonar6.9'
            }
            steps {
              withSonarQubeEnv('sonarserver') {
                sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=JavaSpark \
                                   -Dsonar.projectName=JavaSpark \
                                   -Dsonar.projectVersion=1.0 \
                                   -Dsonar.sources=src/ \
                                   -Dsonar.java.binaries=target/test-classes/ \
                                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
}
     }
}