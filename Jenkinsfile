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

    stage('nohttp') {
        dir('first_app') {
            sh "./mvnw -ntp checkstyle:check"
        }
    }

    stage('install tools') {
        dir('first_app') {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v12.14.0 -DnpmVersion=6.13.7"
        }
    }

    stage('npm install') {
        dir('first_app') {

            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"
        }
    }

    stage('backend tests') {
        dir('first_app') {

            try {
                sh "./mvnw -ntp verify"
            } catch (err) {
                throw err
            } finally {
                junit '**/target/test-results/**/TEST-*.xml'
            }
        }
    }

    stage('frontend tests') {
        dir('first_app') {

            try {
                sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments='run test'"
            } catch (err) {
                throw err
            } finally {
                junit '**/target/test-results/**/TEST-*.xml'
            }
        }
    }

    stage('publish docker') {
        dir('first_app') {

            sh "./mvnw -ntp jib:build"
        }
    }

    stage('Deploy To Registry'){

        docker.withRegistry('http://localhost:5000/firstapp') {
            //sh 'docker push localhost:5000/firstapp:latest'
        }
    }

}
