#!/usr/bin/env groovy

node {
    stage('Récupération des codes') {
            dir('first_app'){
                git branch: '${BRANCHE}', url: 'https://github.com/augustingims/jhipster_jenkinsfile.git'
            }

    }
    stage('Tests and build'){
            dir('first_app') {
                sh "chmod +x mvnw"
                sh "./mvnw -ntp clean"
            }
    }


}
