pipeline{
  agent any
  environment {
    registryBlue = "sumitmann/devops_capstone_blue"
    registryGreen = "sumitmann/devops_capstone_green"
    registryCredential = 'dockercredentials'
    dockerTag = getdockerTag()
  }
  stages{
    stage('Lint HTML'){
      steps{
            sh 'tidy -q -e Blue/index.html'
            sh 'tidy -q -e Green/index.html'
      }
    }
    stage("Lint Dockerfile"){
      agent{
          docker{
              image 'hadolint/hadolint:latest-debian'
          }
        }
      steps {
        sh 'hadolint Blue/Dockerfile.blue | tee -a hadolint_lint.txt'
        sh '''
            lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
            if [ "$lintErrors" -gt "0" ]; then
                echo "Check Error"
                cat hadolint_lint.txt
                exit 1
            else
                echo "No Error"
            fi
        '''
        sh 'hadolint Green/Dockerfile.green | tee -a hadolint_lint.txt'
        sh '''
            lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
            if [ "$lintErrors" -gt "0" ]; then
                echo "Check Error"
                cat hadolint_lint.txt
                exit 1
            else
                echo "No Error"
            fi
        '''
      }
    }
    stage('Build Docker Images'){
      steps{
        sh "docker build -f Blue/Dockerfile.blue Blue -t ${registryBlue}:${dockerTag}"
        sh "docker build -f Green/Dockerfile.green Green -t ${registryGreen}:${dockerTag}"
      }
    }
    stage('Push Docker Images'){
      steps{
        script{
          docker.withRegistry('', registryCredential) {
            sh "docker push ${registryBlue}:${dockerTag}"
            sh '''
                docker tag ${registryBlue}:${dockerTag} ${registryBlue}:latest
            '''
            sh "docker push ${registryBlue}:latest"
            sh "docker push ${registryGreen}:${dockerTag}"
            sh '''
                docker tag ${registryGreen}:${dockerTag} ${registryGreen}:latest
            '''
            sh "docker push ${registryGreen}:latest"
          }
        }
      }
    }
    stage('Set kubectl config') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						kubectl config current-context
						kubectl config use-context arn:aws:eks:eu-west-1:333791965355:cluster/udacity-capstone
					'''
				}
			}
		}
    stage('Blue Deployment') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						kubectl apply -f blue.yaml
					'''
				}
			}
		}
		stage('Green Deployment') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						kubectl apply -f green.yaml	
					'''
				}
			}
		}
    stage('Redirect to Blue') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						kubectl apply -f blue-service.yaml
            kubectl get nodes
            kubectl get deployment
            kubectl get pod -o wide
            kubectl get service/udacity-capstone
					'''
				}
			}
		}
    stage('Wait for user approval') {
      steps {
          input "Redirect the traffic to the green service?"
      }
    }
    stage('Redirect to green') {
			steps {
				withAWS(region:'eu-west-1', credentials:'udacity-capstone') {
					sh '''
						kubectl apply -f green-service.yaml
            kubectl get nodes
            kubectl get deployment
            kubectl get pod -o wide
            kubectl get service/udacity-capstone
					'''
				}
			}
		}
  } 
}

def getdockerTag() {  
  def tag = sh script: 'git rev-parse --short=7 HEAD', returnStdout: true
  return tag.trim()
}