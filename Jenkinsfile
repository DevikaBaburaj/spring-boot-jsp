pipeline {
    agent any

    tools {
        maven '3.8.5'
    }
    
    stages {
        stage('Source') {
            steps {
                git branch: 'batch-4', changelog: false, credentialsId: 'token1', poll: false, url: 'https://github.com/DevikaBaburaj/spring-boot-jsp.git'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Copying Artifcats') {
            environment{
                PUB_KEY = credentials('pubkey')
            }
            steps {
                sh '''
                    version=$(perl -nle 'print "$1" if /<version>(v\\d+\\.\\d+\\.\\d+)<\\/version>/' pom.xml)
                    rsync -avzP target/news-${version}.jar root@52.66.201.38:~/
                '''
            }
        }
    }
}
