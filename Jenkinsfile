/*
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
*/
/*
def gv

pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    gv.buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    gv.buildImage()
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }   
}

*/
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Parallel Tests') {
            parallel {
                stage('Run Unit Tests on Windows') {
                    when {
                        expression { isWindows() }
                    }
                    steps {
                        sh 'mvn test -Dtest.platform=windows'
                    }
                }
                
                stage('Run Unit Tests on Linux') {
                    when {
                        expression { isUnix() }
                    }
                    steps {
                        sh 'mvn test -Dtest.platform=linux'
                    }
                }
            }
        }
        
        stage('Archive') {
            when {
                branch 'develop'
            }
            steps {
                archiveArtifacts artifacts: '*.jar', fingerprint: true
            }
        }
    }
}

