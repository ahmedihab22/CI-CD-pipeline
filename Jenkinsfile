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

    tools {
        // Define Maven installation named 'Maven'
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Use Maven tool to build the project
                script {
                    // This will automatically use the Maven installation defined above
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Parallel Tests') {
            parallel {
                stage('Run Unit Tests on Windows') {
                    when {
                        expression { isWindows() }
                    }
                    steps {
                        // Add your test commands for Windows here
                    }
                }
                
                stage('Run Unit Tests on Linux') {
                    when {
                        expression { isUnix() }
                    }
                    steps {
                        // Add your test commands for Linux here
                    }
                }
            }
        }
        
        stage('Archive') {
            when {
                branch 'develop'
            }
            steps {
                // Archive the built JAR file
                archiveArtifacts artifacts: '*.jar', fingerprint: true
            }
        }
    }
}
