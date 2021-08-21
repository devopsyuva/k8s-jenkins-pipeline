podTemplate(nodeSelector: 'kubernetes.io/hostname=computeplaneone', podRetention: onFailure, namespace: cicd-operations, containers: [ 
    containerTemplate(name: 'kubectl', image: 'ubuntu:20.04', command: 'sleep', args: '99d'), 
    containerTemplate(name: 'testone', image: 'ubuntu:18.04', command: 'sleep', args: '99d') 
  ]) { 
    node(POD_LABEL) { 
        stage('Get testone project') { 
            git 'https://github.com/jenkinsci/kubernetes-plugin.git' 
            container('testone') { 
                stage('Build a testone project') { 
                    sh 'ls -l' 
                } 
            } 
        } 
        stage('Deploy nginx') { 
            git url: 'https://github.com/dubareddy/k8s-jenkins-pipeline.git', branch: 'main' 
            container('kubectl') { 
                stage('Build a testtwo project') { 
                    withKubeConfig([credentialsId: 'gcp_k8s']) { 
                        sh ''' 
                        date 
                        apt update && apt install -y curl iputils-ping iproute2 
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" 
                        chmod +x ./kubectl && mv ./kubectl /usr/local/bin 
                        kubectl get nodes 
                        kubectl get po -A -o wide 
                        kubectl apply -f ./pod/sample-nginx.yaml 
			sleep 20
			kubectl get po -o wide 
                        ''' 
                    } 
                } 
            } 
        } 
    } 
}
