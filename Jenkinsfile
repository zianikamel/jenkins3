pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'lgatica/python-alpine'
                }
            }
            steps {
                sh 'ls'
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'ls'
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python3' 
                }
            }
            steps {
                sh 'cat /var/jenkins_home/workspace/Calculs@tmp/durable-39e8a05e/script.sh'
                sh 'pyinstaller sources/add2vals.py' 
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}
