pipeline {
    agent any
    stages {
        stage('Deploy Application') {
            steps {
                // Configure OpenShift cluster credentials
                withCredentials([string(credentialsId: 'ocp-server-url', variable: 'OCP_SERVER_URL'),
                                  string(credentialsId: 'cluster-auth-token', variable: 'CLUSTER_AUTH_TOKEN'),
                                 
                                  string(credentialsId: 'openshift-cluster-url-2', variable: 'OPENSHIFT_CLUSTER_URL_2'),
                                  string(credentialsId: 'openshift-cluster-token-2', variable: 'OPENSHIFT_CLUSTER_TOKEN_2')]) {
                    
                        // Login to OpenShift cluster 1
                        script{
                        
                        
                       
                        // Deploy application to OpenShift cluster 1
                        try {
                        sh 'oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN && CLUSTER_NAME=$(oc config current-context | cut -d: -f1) && echo Cluster Name: $CLUSTER_NAME && oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN new-project newprojectyx --insecure-skip-tls-verify'
                        
                            sh 'oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN project newprojectyx --insecure-skip-tls-verify && oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN apply -f application-deployment.yaml'
                        
                            // Break out of loop if deployment is successful
                            exit 0
                        } catch (Exception e) {
                            // Log error and continue to next cluster
                            echo "Deployment failed on OpenShift cluster 1. Error: ${e}"
                        }
                        
                        // Login to OpenShift cluster 2
                        
                          
                        // Deploy application to OpenShift cluster 2
                        try {
                         sh 'oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 && CLUSTER_NAME=$(oc config current-context | cut -d: -f1) && echo Cluster Name: $CLUSTER_NAME && oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 new-project newprojectx --insecure-skip-tls-verify'
                           sh 'oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 project newprojectx --insecure-skip-tls-verify && oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 apply -f application-deployment.yaml'
                       
                            // Break out of loop if deployment is successful
                            exit 0
                        } catch (Exception e) {
                            // Log error and continue to next cluster
                            echo "Deployment failed on OpenShift cluster 2. Error: ${e}"
                        }
                        // Log error if deployment fails on all clusters
                        echo "Deployment failed on all OpenShift clusters."
                        }
                        }
                    
                
            }
        }
    }
}
