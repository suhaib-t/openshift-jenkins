pipeline {
    agent { label 'azure-crc' }

    environment {
        OPENSHIFT_SERVER = 'https://api.crc.testing:6443'
        OPENSHIFT_PROJECT = 'default'
        OPENSHIFT_USER = 'kubeadmin'
        OPENSHIFT_PASSWORD = 'vuI2I-95rdJ-wqnwr-Ar2ne'
    }

    stages {
        stage('Login to OpenShift') {
            steps {
                script {
                    // Login to OpenShift using oc CLI
                    sh '''
                    set -e
                    oc login  -u ${OPENSHIFT_USER} ${OPENSHIFT_SERVER} -p ${OPENSHIFT_PASSWORD} --insecure-skip-tls-verify
                    oc project ${OPENSHIFT_PROJECT}
                    '''
                }
            }
        }

        stage('Trigger BuildConfig') {
            steps {
                script {
                    // Trigger the BuildConfig
                    sh 'oc start-build my-node-app-build --wait'
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    // Apply the deployment configuration
                    sh '''
                    set -e
                    oc apply -f /home/suhaib/deployment.yaml
                    # Expose the service if not already exposed
                    # oc expose svc/my-node-app-deployment
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
