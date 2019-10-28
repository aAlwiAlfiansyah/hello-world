#!/usr/bin/env groovy

pipeline {
    // agent {
    //     node {
    //         label 'checker'
    //     }
    // }
    environment {
        // DANGER_GITHUB_API_TOKEN for Quamina
        DANGER_GITHUB_API_TOKEN = retrieveQuaminaToken()
    }
    options {
        skipDefaultCheckout true
    }
    stages {
        stage('Job Checker') {
            steps {
                script {
                    sh "printenv"
                    cancelPreviousJob()
                }
            }
        }
        stage('Checkout') {
            steps {
                script {
                    def scmVars = checkout scm
                    scmVars.each {
                        key, value -> env.setProperty(key, value);
                    }
                    sh "printenv"
                    // checkout([$class: 'GitSCM', 
                    //     branches: [[name: env.CHANGE_BRANCH]],
                    //     doGenerateSubmoduleConfigurations: false,
                    //     extensions: scm.extensions + [
                    //         [$class: 'SparseCheckoutPaths',  sparseCheckoutPaths:[
                    //             [$class:'SparseCheckoutPath', path:'PRChecker'], 
                    //             [$class:'SparseCheckoutPath', path:'Danger'],
                    //             [$class:'SparseCheckoutPath', path:'Dangerfile'],
                    //             [$class:'SparseCheckoutPath', path:'fastlane/*'],
                    //             [$class:'SparseCheckoutPath', path:'Gemfile'],
                    //             [$class:'SparseCheckoutPath', path:'Gemfile.lock']
                    //         ]]
                    //     ],
                    //     userRemoteConfigs: scm.userRemoteConfigs
                    // ])
                }
            }
        }
        stage('Check') {
            options {
                skipDefaultCheckout true
            }
            steps {
                script {
                    sh './Danger'
                }
            }
        }
    }
}

String retrieveQuaminaToken() {
    def token = sh(
            script: "cat ~/quamina_token",
            returnStdout: true
    ).trim()

    // This checking is to make sure that the token is valid token with 40 chars
    if (token.length() == 40) {
        return token
    } else {
        return DANGER_GITHUB_API_TOKEN
    }
}

def cancelPreviousJob() {
    def jobname = env.JOB_NAME
    def buildnum = env.BUILD_NUMBER.toInteger()

    def job = Jenkins.instance.getItemByFullName(jobname)
    for (build in job.builds) {
        if (!build.isBuilding()) {
            continue
        }
        if (buildnum == build.getNumber().toInteger()) {
            continue
            println "equals"
        }
        build.doStop()
    }
}