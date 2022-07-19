pipeline {
    agent any
    stages {
        stage('Static code analysis'){
            steps {
                sh  'python3 -m pylint --output-format=parseable --fail-under=10.0 pystandalonehtml/ --msg-template="{path}:{line}: [{msg_id}({symbol}), {obj}] {msg}" --exit-zero > pylint.log'
                echo 'linting Success, Generating Report'
                 }
            post{
                always{
                    sh 'cat pylint.log'
                    recordIssues enabledForFailure: true, aggregatingResults: true, tool: pyLint(pattern: '**/pylint.log')
                }
            }
        }

        stage('Unit test'){
            steps{
                sh 'pip install -r requirements_dev.txt '
                sh 'python3 -m pytest --junitxml test-results.xml "./tests" '
            }
            post{
                always{
                    junit '**/test-results.xml'
                }
            }
        }
    }
}
