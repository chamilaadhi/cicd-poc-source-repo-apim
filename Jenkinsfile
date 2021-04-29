pipeline {

    agent {
        node {
            label 'master'
        }
    }
    environment { 
        PATH = "/home/chamila_ckad/apictl:$PATH"
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

        stage('Setup Environment for APICTL') {
            steps {
                sh '''
                apictl remove env dev
                apictl add env dev --apim https://localhost:9443 

                '''
            }
        }

        stage('Deploy APIs To "Development" Environment') {
            steps {
                sh '''#!/bin/bash

                apictl login dev -u admin -p admin

                apis=$(apictl vcs status -e dev --format="{{ jsonPretty . }}" | jq -r '.API | .[] | .NickName')
                echo "Updated APIs :"$apis

                rm -rf upload
                mkdir upload
                apiArray=($apis)
                for i in "${apiArray[@]}"
                do
                echo "$i"
                apictl bundle -s $i -d upload
                #curl -u repouser:User@123 -X PUT http://localhost:8082/artifactory/myrepo/ -T PizzaShackAPI_1.0.0.zip
                done
                '''
            }
        }
    }   
}