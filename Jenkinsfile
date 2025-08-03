pipeline{
    agent any
    tools{
        jdk "JDK17"
        maven "MAVEN3.9"
    }
     stages{
        stage('Fetch Code from git'){
            steps{
                git branch: 'develop', url: 'https://github.com/grandeefidel/spark_java_app_ci_cd'
            }
        }
        stage('Run Unit Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Build Stage'){
            steps{
                sh 'mvn install -DskipTests'
            }
            post{
                success{
                    echo "Archiving artifact"
                    archivingArtifacts artifact '**/*.war'
                }
            }
        }
     }
}