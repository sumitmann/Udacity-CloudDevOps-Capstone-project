pipeline{
  agent any
  stages{
    stage('Create a k8s cluster') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						eksctl create cluster \
						--name capstone-cluster-eks \
						--version 1.17 \
						--nodegroup-name capstone-workers-eks \
						--node-type t2.micro \
						--nodes 2 \
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
    stage('Create conf file cluster') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						aws eks --region eu-west-1 update-kubeconfig --name capstone-cluster-eks
					'''
				}
			}
		}
  } 
}