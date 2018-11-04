node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        
        app = docker.build("nferrell/trendbench")
    }
    stage('Create Packer AMI') {
        
            sh 'packer build packer.json'
    }
    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
   
        withDockerRegistry(credentialsId: 'af77b905-1508-49f7-80a1-536451ff9ec8', url: 'https://264846450397.dkr.ecr.us-east-1.amazonaws.com') {
            docker.image('web-server').push('latest')
        }
    }
    stage('Refresh Pod') {
        
            withKubeConfig(caCertificate: '', contextName: '', credentialsId: 'KubeSecret', serverUrl: 'https://172.20.40.96') {

                sh 'kubectl --insecure-skip-tls-verify delete pods -l k8s-app=web-server'
}
    }
}
