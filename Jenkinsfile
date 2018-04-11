pipeline {
    agent any
   //options { skipDefaultCheckout() }
    stages {
        stage ('Trigger_Build_Job') {
            steps {
            script{
           def dateVar = sh returnStdout: true, script: 'date'
           echo "${dateVar}"
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
        stage ('Display_File'){
            steps{
            script{
                //sh returnStdout: true, script: 'docker-compose -f /lgames/docker-compose-all.yml -p testcomp up -d reference'
                def pathadd = sh(script: 'pwd', returnStdout: true)
                echo "this is 111th build ${pathadd}"   
            }
            }
        }  
        stage ('Docker_testing'){
            steps{
            script{
                //sh returnStdout: true, script: 'docker-compose -f /lgames/docker-compose-all.yml -p testcomp up -d reference'
                ///soap/SoapUI-5.4.0/bin/testrunner.sh /data01/jenkins/jobs/banking-enrolment-service-PR/workspace/nectar-openapi-enrolment-soapui-test/New-card-request-resource/NewCardRequest-soapui-project.xml -P Env=T6
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
