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
        sh "./mvnw -ntp checkstyle:check"
    }

    stage('install tools') {
        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v12.14.0 -DnpmVersion=6.13.7"
    }

    stage('npm install') {
        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"
    }

    stage('backend tests') {
        try {
            sh "./mvnw -ntp verify"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/test-results/**/TEST-*.xml'
        }
    }

    stage('frontend tests') {
        try {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments='run test'"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/test-results/**/TEST-*.xml'
        }
    }

    stage('publish docker') {
        sh "./mvnw -ntp jib:build"
    }

}
