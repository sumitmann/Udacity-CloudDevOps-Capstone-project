pipeline{
  agent any
  environment {
    registryBlue = "sumitmann/devops_capstone_blue"
    registryGreen = "sumitmann/devops_capstone_green"
    registryCredential = 'dockercredentials'
    dockerTag = getdockerTag()
  }
  stages{
    // stage('Lint HTML'){
    //   steps{
    //         sh 'tidy -q -e Blue/index.html'
    //         sh 'tidy -q -e Green/index.html'
    //   }
    // }
    // stage("Lint Dockerfile"){
    //   agent{
    //       docker{
    //           image 'hadolint/hadolint:latest-debian'
    //       }
    //     }
    //   steps {
    //     sh 'hadolint Blue/Dockerfile.blue | tee -a hadolint_lint.txt'
    //     sh '''
    //         lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
    //         if [ "$lintErrors" -gt "0" ]; then
    //             echo "Check Error"
    //             cat hadolint_lint.txt
    //             exit 1
    //         else
    //             echo "No Error"
    //         fi
    //     '''
    //     sh 'hadolint Green/Dockerfile.green | tee -a hadolint_lint.txt'
    //     sh '''
    //         lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
    //         if [ "$lintErrors" -gt "0" ]; then
    //             echo "Check Error"
    //             cat hadolint_lint.txt
    //             exit 1
    //         else
    //             echo "No Error"
    //         fi
    //     '''
    //   }
    // }
    // stage('Build Docker Image'){
    //   steps{
    //     sh "docker build -f Blue/Dockerfile.blue Blue -t ${registryBlue}:${dockerTag}"
    //     sh "docker build -f Green/Dockerfile.green Green -t ${registryGreen}:${dockerTag}"
    //   }
    // }
    // stage('Push Docker Image'){
    //   steps{
    //     script{
    //       docker.withRegistry('', registryCredential) {
    //         sh "docker push ${registryBlue}:${dockerTag}"
    //         sh '''
    //             docker tag ${registryBlue}:${dockerTag} ${registryBlue}:latest
    //         '''
    //         sh "docker push ${registryBlue}:latest"
    //         sh "docker push ${registryGreen}:${dockerTag}"
    //         sh '''
    //             docker tag ${registryGreen}:${dockerTag} ${registryGreen}:latest
    //         '''
    //         sh "docker push ${registryGreen}:latest"
    //       }
    //     }
    //   }
    // }
    // stage('Create cluster') {
		// 	steps {
		// 		withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
		// 			sh '''
		// 				eksctl create cluster \
		// 				--name capstonecluster \
		// 				--version 1.17 \
		// 				--nodegroup-name standard-workers \
		// 				--node-type t2.micro \
		// 				--nodes 2 \
		// 				--nodes-min 1 \
		// 				--nodes-max 3 \
		// 				--node-ami auto \
		// 				--region eu-west-1 \
		// 				--zones eu-west-1a \
		// 				--zones eu-west-1b \
		// 				--zones eu-west-1c \
		// 			'''
		// 		}
		// 	}
		// }
    // stage('Create conf file cluster') {
		// 	steps {
		// 		withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
		// 			sh '''
		// 				aws eks --region eu-west-1 update-kubeconfig --name capstonecluster
		// 			'''
		// 		}
		// 	}
		// }
    stage('Deployment-Blue') {
      steps {
          withAWS(credentials: 'udacity-capstone', region: 'eu-west-1') {
              sh 'kubectl apply -f blue-controller.yaml'
              sh 'kubectl apply -f service-controller-blue.yaml'
              sh 'kubectl get services blue'
          }
      }
    }
    stage('Deployment-Green') {
      when {
          branch 'Deployment-Blue'
      }
      steps {
          withAWS(credentials: 'udacity-capstone', region: 'eu-west-1') {
              sh 'kubectl apply -f green-controller.yaml'
              sh 'kubectl apply -f service-controller-blue.yaml'
              sh 'kubectl get services green'
          }
      }
    }
  } 
}

def getdockerTag() {  
  def tag = sh script: 'git rev-parse --short=7 HEAD', returnStdout: true
  return tag.trim()
}