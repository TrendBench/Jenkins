node {
    def app

    stage('Clone Repository') {
        /* Clone repository to workspace */

        checkout scm
    }

    stage('Build Docker Image') {
        /* Build Dockler Image */
        
        app = docker.build("trendbench")
    }
    stage('Build AMI (optional)') {
        
           /* sh 'packer build packer.json' */
    }

    stage('Push Docker Image') {
   
        docker.withRegistry("https://264846450397.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:ecr-credentials") {
            docker.image("trendbench").push('latest')
}
    }
    stage('Test Stage') {

            sh '/var/lib/jenkins/sc/scan.py'
    }  
    stage('Refresh Nodes') {
        
            withKubeConfig(caCertificate: '', contextName: '', credentialsId: 'KubeSecret', serverUrl: 'https://172.20.40.96') {

                sh 'kubectl --insecure-skip-tls-verify delete pods -l k8s-app=web-server'
}
    }
}
