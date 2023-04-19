pipeline {
    agent any
    stages {
        stage('Deploy Application') {
            steps {
                // Configure OpenShift cluster credentials
                withCredentials([string(credentialsId: 'ocp-server-url', variable: 'OCP_SERVER_URL'),
                                  string(credentialsId: 'cluster-auth-token', variable: 'CLUSTER_AUTH_TOKEN')]){
                        // Login to OpenShift cluster 1
                        script{
                        // Deploy application to OpenShift cluster 1
                            try{
                            def clusterStatusExitCode = sh(script: 'oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN get nodes', returnStatus: true)
def clusterStatusOutput = sh(script: 'oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN get nodes', returnStdout: true)

                            }
                            catch(Exception e){
                                echo "authentication failed"
                            }
                            //authentication success
                            if(clusterStatusExitCode==0){
                    if (!clusterStatusOutput.contains('NotReady')) {
                        

                        try {
  

                        
                            sh 'oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN project newprojectg --insecure-skip-tls-verify && oc --server=$OCP_SERVER_URL --token=$CLUSTER_AUTH_TOKEN apply -f application-deployment.yaml'
                            // Break out of loop if deployment is successful
                        } catch (Exception e) {
                            // Log error and continue to next cluster
                            echo "Deployment failed on OpenShift cluster 1. Error: ${e}"
                            
                          
                        }
                        }
                
            
            
            else{
         
            echo "Cluster is not ready"
            env.deploy_on_failure = 'true'
            }
                        }
                            
                            //authenitcation failed
                        
                    else{
                        echo "authentication failed"
                    }
                        }
                                  }
            }      // Login to OpenShift cluster 2
                        }
                    stage('deploy to cluster2'){
                    when{
                       anyOf {
                       
            environment name: 'DEPLOY_ON_FAILURE', value: 'true'
        }
        }
                        // Deploy application to OpenShift cluster 2
                        steps{
                        
                                  withCredentials([string(credentialsId: 'openshift-cluster-url-2', variable: 'OPENSHIFT_CLUSTER_URL_2'),
                                  string(credentialsId: 'openshift-cluster-token-2', variable: 'OPENSHIFT_CLUSTER_TOKEN_2')]) {
                            script{
                        try {
                                             
    
                         sh 'oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 && CLUSTER_NAME=$(oc config current-context | cut -d: -f1) && echo Cluster Name: $CLUSTER_NAME && oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 new-project newprojectx --insecure-skip-tls-verify'
                           sh 'oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 project newprojectx --insecure-skip-tls-verify && oc --server=$OPENSHIFT_CLUSTER_URL_2 --token=$OPENSHIFT_CLUSTER_TOKEN_2 apply -f application-deployment.yaml'
                            // Break out of loop if deployment is successful
                        } catch (Exception e) {
                            // Log error and continue to next cluster
                            echo "Deployment failed on OpenShift cluster 2. Error: ${e}"
                        }
                        // Log error if deployment fails on all clusters
                        
                        }
                        }
                }
                }
                }
            }
            






