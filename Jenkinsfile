def label = "worker-${UUID.randomUUID().toString()}"
echo label

podTemplate (containers: [
                          containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
                          containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.0', command: 'cat', ttyEnabled: true),
                          containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
                         ],
             volumes: [
                        hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
                      ]) 
       {
        node(POD_LABEL) {
            stage('Checkout') {
                       echo 'Checking out project from Git...'
                       git branch : 'master', credentialsId: '34d67bef-0861-473b-9714-79d9331d5e80', url : 'https://github.com/TuneableSloth/SampleTomcatApp.git'
                    }
               
            stage('Package'){
                echo 'Building war file...'
                sh script: "jar -cf app.war ."
                    }
 
            stage('Build') {
                container('docker') {
                       echo 'Building docker image...'
                       sh "docker build -t prg/app_latest ."
                       }
                    }
            
            stage('Publish') {
                container('docker') {
                     withDockerRegistry([credentialsId: '909b46b0-1446-4777-8695-9ae742d4b9f0', url: ""]) {
                        sh "docker tag prg/app_latest prgbaw/myrepo:latest" 
                        sh "docker push prgbaw/myrepo:latest"
                        }
                }
            }
            stage('Deploy') {
                container('helm') {
                    sh "helm upgrade --install -n jenkins applicationchart --set global.version=latest ./helm"
            }
        }    
}
}

