pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1 
kind: Pod 
spec: 
  containers: 
  - name: centos 
    image: centos 
    command: 
    - sleep 
    args: 
    - 99d 
  restartPolicy: Never 
            '''
        }
    }
    stages {
        stage('Deployment: Adjust replica') {
            steps {
                sh '''
                curl -k -v -XPATCH -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  -H "Accept: application/json" -H "Content-Type: application/strategic-merge-patch+json" -H "User-Agent: kubectl/v1.21.5 (darwin/amd64) kubernetes/aea7bba" 'https://10.96.0.1:443/apis/apps/v1/namespaces/staging/deployments/calculator-deployment?fieldManager=kubectl-client-side-apply' --data '{"spec":{"replicas":2,"template":{"spec":{"containers":[{"name":"calculator","image":"dlambrig/week8:1.1"}]}}}}'
                '''
            }
            post {
                success {
                    echo 'Deployment finished'
                }
            }
        }
    }
}