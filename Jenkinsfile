node {
        stage ("Build"){
            echo '*** Build Starting ***'

            openshiftBuild bldCfg: 'cats', checkForTriggeredDeployments: 'false', commitID: '', namespace: 'cotd-dev', showBuildLogs: 'false', verbose: 'false'
            openshiftVerifyBuild bldCfg: 'cats', checkForTriggeredDeployments: 'false', namespace: 'cotd-dev', verbose: 'false'


            echo '*** Build Complete ***'
        }

        stage ("Promote image to Test"){

            openshiftTag(namespace: 'cotd-dev', srcStream: 'cats', srcTag: 'latest', destStream: 'cats', destTag: 'testready')


        }
            
        stage ("Deploy and Verify in Test Env"){
            echo '*** Deployment Starting ***'
            openshiftDeploy depCfg: 'cats', namespace: 'cotd-test', verbose: 'false', waitTime: ''
            openshiftVerifyDeployment depCfg: 'cats', namespace: 'cotd-test', replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: ''
            echo '*** Deployment Complete ***'

            }

   stage ("Promote image to Production"){

            openshiftTag(namespace: 'cotd-dev',srcStream: 'cats', srcTag: 'testready', destStream: 'cats', destTag: 'prodready')


        }
        stage ("Deploy and Verify in Prod Env"){


            echo '*** Deployment Starting ***'
            echo '*** Waiting for Input ***'
            input 'Should we deploy to Production?' 
            openshiftDeploy depCfg: 'cats', namespace: 'cotd-prod', verbose: 'false', waitTime: ''
            openshiftVerifyDeployment depCfg: 'cats', namespace: 'cotd-prod', replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: ''
            echo '*** Deployment Complete ***'

        }

} 
