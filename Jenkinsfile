pipeline{
    agent any

    parameters {
            choice choices: ['chrome', 'firefox'], name: 'Browser'
        }

    stages{
        stage("Start Grid"){
            steps{
                sh "docker-compose -f grid.yaml up --scale ${params.Browser}=2 -d"
            }
        }
        stage("Execute Test Suites"){
            steps{
                sh "docker-compose -f features.yaml up --pull=always"
            }
        }
        }

    post{
        always{
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f features.yaml down"
            archiveArtifacts artifacts: 'docker-output/results.html', followSymlinks: false
            archiveArtifacts artifacts: 'docker-output/testLogs.log', followSymlinks: false
        }
    }
}