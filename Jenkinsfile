pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Prepare') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Prepare'
                dir('code/backend'){
                    sh 'npm install'
                }
                dir('code/frontend'){
                    sh 'npm install'
                }
            }
        }
        stage('Build') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Build'  
                dir('code/backend'){
                    sh 'npm i'
                    sh 'npm b'    
                }
                dir('code/frontend'){
                    sh 'npm i'
                    sh 'npm b'    
                }
            }
        }
        stage('Static Analysis') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Analyze' 
                dir('code/backend'){
                    sh 'npm lint'  
                }
                dir('code/frontend'){
                    sh 'npm lint'  
                }
            }
        }
        stage('Unit Test') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                echo 'Test'
                dir('code/backend'){
                    sh 'npm test'  
                }
                dir('code/frontend'){
                    sh 'npm test'  
                }
            }
        }
        stage('e2e Test') {
            steps {             
                echo 'e2e Test'
                sh 'docker-compose -f docker-compose.yml build'
                sh 'docker-compose -f docker-compose-e2e.yml build'
                sh 'docker-compose -f docker-compose.yml up -d'
                sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend'
                sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v'
                script {
                    sh 'docker-compose -f docker-compose-e2e.yml up e2e'
                    status_code = echo ( script: "docker inspect code_e2e_1 --format='{{.State.ExitCode}}'", returnStdout: true).trim();
                    if (status_code == '1'){
                        error('e2e test failed.')
                    }
                }
            }
            post {
                always {
                    echo 'Cleanup'
                }
            }
        }
        stage('Deploy') {
            steps {                
                echo 'Deploy'
            }
        }
        stage('TestStage') {
            steps {                
                echo 'Dancing'
            }
        }
    }
}
