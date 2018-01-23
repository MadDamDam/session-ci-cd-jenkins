#!/usr/bin/env groovy
pipeline {
    // options([pipelineTriggers([githubPush()])])

    //triggers {
    //    pipelineTriggers(
    //    )
    //    }

    agent {label 'slave_node'}

    environment {PATH = "$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH"}

    stages {

//        stage ('Checkout') {
//            checkout(
//              [$class: 'GitSCM',
//                branches: [[name: '*/master']],
//                userRemoteConfigs: [[credentialsId: 'f1b23df2-9714-46fc-a220-d47d41406718',
//                  url: 'git@github.com:mickeysh/session-ci-cd-jenkins']]]
//            )
//        }


//        stage('Checkout') { // for display purposes
//            steps{
//                // Get code from the GitHub repository
//                git 'https://github.com/MadDamDam/session-ci-cd-jenkins.git'
//                echo 'finished stage get_src'
//            }
//       }

        stage('Test') {
            //when{} block is like an if in declarative. decides when the stage runs.
            //Declarative sucks!
            when {
                    expression { return isUnix()}
            }
            steps{
                    echo "This is unix!"
                    sh 'echo $PATH'
                    //withEnv(["PATH+EXTRA=${env.HOME}/.rbenv/bin:${env.HOME}/.rbenv/shims"]) {
                    sh 'echo $PATH'
                    sh 'ruby app/tc_ruby_app.rb'
                //}

                    sh 'ls -ltrh test/reports/'
            }
        }

        stage('Build') {
            steps{
                // build docker stage
                sh 'sudo docker build --no-cache -t localhost:5000/opsschool_dummy_app:latest .'
                sh 'sudo docker push localhost:5000/opsschool_dummy_app:latest'
                sh 'sudo docker-compose stop'
                script {
                    def dockerPSoutput = sh (
                        script: 'sudo docker ps -q -f name=opsschool_dummy_app',
                        returnStdout: true
                        ).trim()
                    echo "dockerPSoutput is:${dockerPSoutput}."
                    if (dockerPSoutput) {
	                    sh "sudo docker rm -f opsschool_dummy_app"
	               }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'sudo docker pull localhost:5000/opsschool_dummy_app:latest'
                sh 'sudo docker-compose up -d'
            }
        }
    }

    post {
        always {
            junit 'test/reports/*.xml'
        }
    }
}
