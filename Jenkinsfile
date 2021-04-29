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
                sh """#!/bin/bash
                ENVCOUNT=\$(apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "1" ]; then
                    apictl add-env -e dev --apim https://localhost:9444
                fi
                """
            }
        }

        stage('Deploy APIs To "Development" Environment') {
            steps {
                sh """
                apictl login dev -u devops -p devops
                apis=$(apictl vcs status -e dev --format="{{ jsonPretty . }}" | jq -r '.API | .[] | .NickName')
                echo "Updated APIs :"$apis
                #apictl vcs deploy -e dev
                """
            }
        }
    }   
}