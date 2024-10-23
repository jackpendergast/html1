node('ubuntu-AppServer-3120')
{

def app
stage('Cloning Github')
{
    /*cloning the GitHub repository*/
    checkout scm
}

stage('Build and Tag Image')
{
    /*build the docker image and append a tag at the end*/
    
    app = docker.build('penjack/car_docker_repo')
}

stage('Post Image to Dockerhub')
{
    /*push Image to DockerHub Repository*/
    
    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
    {
        app.push('latest')
    }
}

stage('Prepare Environment') {
        // Find and stop any Docker container using port 80
        sh '''
        CONTAINER_ID=$(docker ps -q --filter "publish=80")
        if [ -n "$CONTAINER_ID" ]; then
            echo "Stopping container using port 80: $CONTAINER_ID"
            docker stop $CONTAINER_ID
            docker rm $CONTAINER_ID
        else
            echo "No container using port 80."
        fi
        '''
    }

stage('Deploy')
{
    sh 'docker-compose down --remove-orphans'
    sh 'docker-compose up -d'
    
}
}
