pipeline {
    agent any
   //options { skipDefaultCheckout() }
    stages {
        stage ('Trigger_Build_Job') {
            steps {
            script{
           // def dateVar = sh returnStdout: true, script: 'date'
           // echo "${dateVar}"
            build job: 'TestFreeStyle', parameters: [[$class: 'StringParameterValue', name : 'parameterOne' , value: params.module ]]
            
            }
            }
            post{
                success{
                    echo "Job execution Success"
                }
                failure{
                    build job:'Send_Email',parameters: [[$class: 'StringParameterValue', name : 'message' , value: "Build Job failed" ]],propagate:false
                    
                }
            }
            }
        stage ('Trigger_jmeter_Job'){
            steps{
            echo params.PerfTestOnT0
            script{
               sh 'exit 0'
            }
            }
            post{
                success{
                    echo "Job execution Success"
                }
                failure{
                    build job:'Send_Email',parameters: [[$class: 'StringParameterValue', name : 'message' , value: "jMeter failed" ]],propagate:false
                    
                }
            }
        }
        stage ('Docker_testing'){
            steps{
            script{
                //sh returnStdout: true, script: 'docker-compose -f /lgames/docker-compose-all.yml -p testcomp up -d reference'
                //sh(script: 'docker-compose -f /lgames/docker-compose-all.yml -p testcomp down', returnStdout: true)
                echo "Docker up and down working"   
            }
            }
        }
        stage ('Functional_Test_T4/T6'){
            steps{
                script{
                    if  ( params.FunctionalTestSrv == 'T4' )
                    {
                     sh 'echo "add ansible command here to push and run test on T4"'
                    }
                    else {
                     sh 'echo "add ansible command here to push and run test on T6"'            
                    }
                    }
            }
        }
        stage ('Performance_Test_T0'){
            steps{
                script{
                    if  ( params.PerfTestOnT0 == 'Needed' )
                    {
                     sh 'echo "add ansible command here to push and run test on T0"'
                    }
                    else {
                     sh 'echo "Performace test is omitted"'            
                    }
                    }
            }
        }
        }
    }
