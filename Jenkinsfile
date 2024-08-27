pipeline {
    agent any
    
    environment {
        OPENSHIFT_SERVER = 'https://api.crc.testing:6443'
        OPENSHIFT_PROJECT = 'default'
        OPENSHIFT_USER = 'kubeadmin'
        OPENSHIFT_PASSWORD = 'vuI2I-95rdJ-wqnwr-Ar2ne'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from Git repository
                checkout([$class: 'GitSCM',
                          userRemoteConfigs: [[url: 'https://github.com/suhaib-t/openshift-jenkins.git']]])
            }
        }

        stage('Login to OpenShift') {
            steps {
                script {
                    // Login to OpenShift using oc CLI
                    sh "oc login ${env.OPENSHIFT_SERVER} -u ${env.OPENSHIFT_USER} -p ${env.OPENSHIFT_PASSWORD} --insecure-skip-tls-verify"
                    
                    // Switch to the desired project
                    sh "oc project ${env.OPENSHIFT_PROJECT}"
                }
            }
        }
        
        stage('Trigger BuildConfig') {
            steps {
                script {
                    // Trigger the BuildConfig
                    sh "oc start-build my-node-app-build --wait"
                }
            }
        }
        
        stage('Deploy Application') {
            steps {
                script {
                    // Apply the deployment configuration
                    sh "oc apply -f deployment.yaml"
                    
                    // Expose the service if not already exposed
                    //sh "oc expose svc/my-node-app-deployment "
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
