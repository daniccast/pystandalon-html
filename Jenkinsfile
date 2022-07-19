pipeline {
    agent any
    stages {
        stage('Static code analysis'){
            steps {
                sh  'python3 -m pylint --output-format=parseable --fail-under=10.0 pystandalonehtml/ --msg-template="{path}:{line}: [{msg_id}({symbol}), {obj}] {msg}"  > pylint.log'
                echo 'linting Success, Generating Report'
                 }
            post{
                always{
                    sh 'cat pylint.log'
                    recordIssues enabledForFailure: true, aggregatingResults: true, tool: pyLint(pattern: '**/pylint.log')
                }
            }
        }
    }
}
