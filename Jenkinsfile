pipeline {
    agent { label 'ubuntu_agent' }
    triggers {
        pollSCM("")
    }
    stages {
        stage('info') {
            steps {
                sh 'npm -v'
                sh 'node -v'
                sh 'pwd'
                sh 'ls -la'
            }
        }
        stage('Build') {
	          steps {
                sh 'npm config set registry http://192.168.0.75:8081/repository/nexus-npm/'
                sh 'npm config set loglevel verbose'	
                sh 'npm install'
            }
        }
        stage('Test') {
	          steps {
                sh 'echo npm run test'
            }
        }
        stage('Deploy') {
            steps {
               script {
                    ansiblePlaybook credentialsId: 'pradeep-rhel', inventory: 'deploy/hosts', playbook: 'deploy/site.yml', sudoUser: null
                }
            }
        }
    }  	    
	    
    post {
        always {
            sh 'echo tests'
        }
    }
}
