pipeline {
    agent {
        label 'antora-asciidoc'
    }

    stages {

      stage('Install Yarn') {
          steps {
                sh 'npm i -g yarn'
                sh 'yarn install'
          }
      }

      stage('Build Site') {
            steps {
                    sh 'antora --generator=@antora/site-generator-ms sap-workshop.yml'
                }
        }

        // stage('Configure aws') {
        //     steps {
        //         sh 'export AWS_ACCESS_KEY_ID=$AWS_CREDS_USR'
        //         sh 'export AWS_SECRET_ACCESS_KEY=$AWS_CREDS_PSW'
        //         sh 'export AWS_DEFAULT_REGION=us-east-1'
        //         sh '$(aws ecr get-login --no-include-email --region us-east-1)'
        //     }
        // }

        // stage('Build Docker Container') {
        //     steps {
        //         configFileProvider([configFile(fileId: "Dockerfile", replaceTokens: true, targetLocation: './Dockerfile')]) {
        //             script{
        //                 def sapImage = docker.build("sap-workshop")
        //             }
        //         }
        //     }
        // }

        // stage('Publish Docker container') {
        //     steps {
        //         sh 'docker login repo.treescale.com -u demos_kb8 -p Mule1379'
        //         sh 'docker tag sap-workshop:latest 567617630570.dkr.ecr.us-east-1.amazonaws.com/sap-workshop:latest'
        //         sh 'docker push 567617630570.dkr.ecr.us-east-1.amazonaws.com/sap-workshop:latest'
        //     }
        // }

        stage('Copy site to server') {
            steps {
                withCredentials([file(credentialsId: 'se-team.pem', variable: 'PEM_FILE')]){
                    sh 'cp "$PEM_FILE" .'
                    sh 'echo "pem copied."'
                    sh 'chmod 400 ./se-team.pem'
                    sh 'ls -ltrh build/site'
                    sh 'scp -o "StrictHostKeyChecking no" -r -i ./se-team.pem build/site ec2-user@34.227.143.105:/usr/share/nginx'
                }
            }
        }
    }
}
