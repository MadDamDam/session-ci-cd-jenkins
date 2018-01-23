pipeline {
    // options([pipelineTriggers([githubPush()])])

    //triggers {
    //    pipelineTriggers(
    //    )
    //    }

    agent {label 'slave_node'}

    environment {PATH = "$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH"}

    stages {
        stage('Get_src') { // for display purposes
            steps{
                //def mvnHome
                // Get some code from a GitHub repository
                git 'https://github.com/MadDamDam/session-ci-cd-jenkins.git'
                //mvnHome = 'DamDam here!'
                echo 'finished stage get_src'
            }
       }

        stage('Run_ruby') {
            when {
                    expression { return isUnix()}
            }
            steps{
                // Run ruby pre step
                //if (isUnix()) {
                    //echo "${mvnHome} This is unix!"
                    echo "This is unix!"
                    sh 'echo $PATH'
                    //withEnv(['PATH=$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH']) {
                    //withEnv(["PATH+EXTRA=${env.HOME}/.rbenv/bin:${env.HOME}/.rbenv/shims"]) {
                    sh 'echo $PATH'
                    sh 'ruby app/tc_ruby_app.rb'
                //}
            // sh 'ruby app/tc_ruby_app.rb'
                    sh 'ls -ltrh test/reports/'

            //sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                //} else {
                //    echo "I don't know what to do if this isn't unix"
                //}
            }
        }

        stage('Build_docker') {
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

        stage('Run_build') {
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
