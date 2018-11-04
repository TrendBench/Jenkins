node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        
        app = docker.build("trendbench")
    }
    stage('Create Packer AMI') {
        
            /*sh 'packer build packer.json'*/
    }
    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
   
        docker.withRegistry("https://264846450397.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:ecr-credentials") {
            docker.image("web-server").push('latest')
}
    }
    stage('Refresh Pod') {
        
            withKubeConfig(caCertificate: '', contextName: '', credentialsId: 'KubeSecret', serverUrl: 'https://172.20.40.96') {

                sh 'kubectl --insecure-skip-tls-verify delete pods -l k8s-app=web-server'
}
    }
}
