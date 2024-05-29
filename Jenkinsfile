pipeline {
    agent any

    tools {
        nodejs 'nodejs-22'
    }

    stages {
        stage('Build and Push') {
            steps {
                echo 'Building and pushing'
                sh 'docker build -t ttl.sh/docker-node-frank .'
                sh 'docker push ttl.sh/docker-node-frank'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to target'
                withCredentials([sshUserPrivateKey(credentialsId: 'mykey',
                                                   keyFileVariable: 'mykey',
                                                   usernameVariable: 'myuser')]) {
                    
                    script {
                        // Stop and remove containers
                        sh "ssh vagrant@192.168.105.3 -i ${mykey} \"docker stop myapp\""
                        sh "ssh vagrant@192.168.105.3 -i ${mykey} \"docker rm myapp\""
                    }
                    
                    sh "ssh vagrant@192.168.105.3 -i ${mykey} \"docker run -d -p 4444:4444 --name myapp ttl.sh/docker-node-frank\""
                }
            }
        }
    }
}