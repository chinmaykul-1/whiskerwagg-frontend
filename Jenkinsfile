pipeline{
    agent any
     environment {
        EC2_HOST = "ec2-user@13.233.150.180"
        PRIVATE_KEY = credentials('ec2-creds')
        DOCKER_USER = "chinmaykulkarni19"
        DOCKER_PASS = credentials('docker-hub-creds') 
    }

    stages{
        stage('Checkout code'){
            steps{
                 git branch: 'main', url: 'https://github.com/chinmaykul-1/whiskerwagg-frontend'
            }
        }
        stage('Logon to aws node and pull image'){
            steps{
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-creds', keyFileVariable: 'SSH_KEY'),
                usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')])
                {
                    sh """
                    ssh  -i $SSH_KEY $EC2_HOST << EOF
                    echo "connected to ec2 node"
                    sudo su -
                    sudo echo $DOCKER_PASS | sudo docker login -u $DOCKER_USER --password-stdin
                    sudo docker pull chinmaykulkarni19/whiskerwagg-frontend
                    sudo docker stop frontend || true
                    sudo docker rm frontend || true
                    sudo docker run -d -p 5173:5173 --name frontend chinmaykulkarni19/whiskerwagg-frontend
                    """
                }

            }
        }

    }
}