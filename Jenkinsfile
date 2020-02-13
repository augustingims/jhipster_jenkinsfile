#!/usr/bin/env groovy

node {
    stage('Récupération des codes') {
        steps{
            dir('first_app'){
                git branch: '${BRANCHE}', url: 'https://github.com/augustingims/jhipster_jenkinsfile.git'
            }
        }
    }
    stage('Tests and build'){
        steps{
            dir('first_app') {
                bat "mvn clean install"
            }
        }
    }


}
