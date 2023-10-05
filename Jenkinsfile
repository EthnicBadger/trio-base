pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is working"
                '''
            }
        }
      stage('Build Images') {
            steps {
                sh ''' 
                echo "Hello, Starting the image build process"

                # BUILDING EACH IMAGE IN TURN
                # LATEST PLUS A VERSION NUMBERED IMAGE

                docker build -t ethnicbadger/trio-base-app:latest ./flask-app
                docker build -t ethnicbadger/trio-base-app:${BUILD_NUMBER} ./flask-app

                docker build -t ethnicbadger/trio-base-db:latest ./db
                docker build -t ethnicbadger/trio-base-db:${BUILD_NUMBER} ./db

                '''
            }
        }
        stage('Push Images') {
            steps {
                sh ''' 
                echo "Hello, Starting to push the images to DockerHub"

                # PUSH THE NEW IMAGES TO DOCKER HUB

                docker push ethnicbadger/trio-base-app:latest
                docker push ethnicbadger/trio-base-app:${BUILD_NUMBER}

                docker push ethnicbadger/trio-base-db:latest
                docker push ethnicbadger/trio-base-db:${BUILD_NUMBER}
                '''
            }
        }

        stage('clean up Jenkins') {
            steps {
                sh ''' 
                echo "Hello, Starting clean up of Jenkins"

                docker rmi ethnicbadger/trio-base-app:latest
                docker rmi ethnicbadger/trio-base-app:${BUILD_NUMBER}

                docker rmi ethnicbadger/trio-base-db:latest
                docker rmi ethnicbadger/trio-base-db:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Containers') {
            steps {
                sh ''' 
                echo "Hello, Jenkins is still working"
                # NOW WE'RE GOING TO DEPLOY THE CONTAINERS BASED ON THE IMAGES GENERATED
                cd ~
                cd ./trio-base
                git pull
                cd ./k8s
                kubectl rollout restart deployment --namespace=pre-prod flask-deployment
                '''
            }
        }
        
    }
}
