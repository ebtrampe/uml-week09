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
                curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments PATCH -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
                curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/staging/deployments PATCH -H "Content-type: application/yaml" --data-binary @calculator.yaml
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