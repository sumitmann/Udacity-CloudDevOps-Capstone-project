pipeline {
    agent any
    
    stages{

        stage('Creating Kubernetes Cluster') {
            steps {
                withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
                    sh '''
                            eksctl create cluster \
                            --name udacity-capstone \
                            --version 1.17   \
                            --nodegroup-name udacity-capstone-nodes \
                            --node-type t2.micro \
                            --nodes 3 \
                            --nodes-min 1 \
                            --nodes-max 3 \
                            --node-ami auto \
                            --region eu-west-1 \
                            --zones eu-west-1a \
                            --zones eu-west-1b \
                            --zones eu-west-1c \
                        '''
                }
            }
        }

        stage('Create cluster configuration') {
            steps {
                withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
                    sh 'aws eks --region eu-west-1 update-kubeconfig --name udacity-capstone'
                }
            }
        }
    }
}