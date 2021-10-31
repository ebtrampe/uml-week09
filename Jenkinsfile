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
                curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments?fieldManager=kubectl-client-side-apply -XPATCH -H "Content-type: application/apply-patch+yaml" --data-binary @hazelcast.yaml
                curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments?fieldManager=kubectl-client-side-apply -XPATCH -H "Content-type: application/apply-patch+yaml" --data-binary @calculator.yaml
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