
// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python
    command:
    - sleep
    args:
    - infinity
 
'''
          defaultContainer 'python'
        }
    }
    
    stages {
        stage('Build') {
            steps ('Install dependencies'){
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run unit tests'){
            steps {
                sh 'pytest testRoutes.py'
            
            }
        }
        stage('Generate artifact'){
            steps {
                sh 'ls -lha'
                sh 'pwd'
                sh 'mkdir artifacts'
                sh "tar -czvf artifacts/python-app:${env.BUILD_ID}.tar.gz ."
            
            }
        }
        stage('Upload artifact'){
            post {
                success {
                    archiveArtifacts artifacts: 'artifacts/*', fingerprint: true
                }
                
            }
        }
    }
}
