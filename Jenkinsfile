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
        stage('Deployment') {
            steps {
                sh '''
                curl -k -v -XPATCH -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  -H "Accept: application/json" -H "Content-Type: application/strategic-merge-patch+json" -H "User-Agent: kubectl/v1.21.5 (darwin/amd64) kubernetes/aea7bba" 'https://kubernetes.docker.internal:6443/apis/apps/v1/namespaces/staging/deployments/calculator-deployment?fieldManager=kubectl-client-side-apply' --data '{"spec":{"replicas":2}}
                curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml
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