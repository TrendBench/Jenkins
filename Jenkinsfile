node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build Docker image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        
        app = docker.build("trendbench")
    }
    stage('Build AMI') {
        
           /* sh 'packer build packer.json' */
    }

    stage('Push Docker image') {
   
        docker.withRegistry("https://264846450397.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:ecr-credentials") {
            docker.image("trendbench").push('latest')
}
    }
    stage('Test Stage') {

            sh '/var/lib/jenkins/sc/scan.py'
    }  
    stage('Refresh Pods') {
        
            withKubeConfig(caCertificate: '', contextName: '', credentialsId: 'KubeSecret', serverUrl: 'https://172.20.40.96') {

                sh 'kubectl --insecure-skip-tls-verify delete pods -l k8s-app=web-server'
}
    }
}
