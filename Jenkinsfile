pipeline{
    agent any
    stages{
        stage('Poll Config file changes from GitHub')
        {
            steps{
                echo 'Fetch Config file changes from GitHub'
                git branch: 'main', credentialsId: 'df8d2f9a-b1f5-43ae-a31c-fa4e163c1bf4', url: 'https://github.com/shibkali-bera/POC_GIT_SNOW_Integration.git'
            }
        }
        stage('Pushing to Artifactory')
        {
            steps{
                rtServer (
                    id: 'Artifactory-1',
                    url: 'http://54.160.21.79:8082//artifactory',
                    username: 'admin',
                    password: 'Devops2020',
                    timeout: 300
                )
                rtUpload (
                    serverId: 'Artifactory-1',
                    spec: '''{
                          "files": [
                            {
                              "pattern": "$WORKSPACE/*.txt",
                              "target": "example-repo-local/$BUILD_ID $JOB_NAME/"
                            }
                          ]
                    }'''
                )
            }
        }
        
        stage('Create Incident In SNOW')
        {
            steps{
                echo 'Creating Incident ID'
                sh 'echo \"{\'short_description\':\'`date` Test Incident Short Description\',\'urgency\':\'3\',\'impact\':\'2\'}\" > mydata.json'
		sh 'curl https://dev51182.service-now.com/api/now/table/incident -X POST -H Accept:application/json -H Content-Type:application/json --data @mydata.json --user admin:gVlCBvfE4aZ1'
                //echo '=========================Response===================' + response
            }
        }
        
        stage('Clean Workspace')
        {
            steps{
                sh 'rm -rf $WORKSPACE/*'
            }
        }
    }
}
