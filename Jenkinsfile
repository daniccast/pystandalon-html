pipeline {
    agent any
    stages {
        stage('Static code analysis'){
            steps {
                sh  'python3 -m pylint --output-format=parseable --fail-under=10.0 pystandalonehtml-marinions/ --msg-template="{path}:{line}: [{msg_id}({symbol}), {obj}] {msg}" --exit-zero > pylint.log'
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
        stage('Deployment testpypi'){
            steps{
                sh 'pip install twine'
                sh 'rm -rf dist'
                sh 'python3 setup.py sdist bdist_wheel'
                sh 'python3 -m twine upload --repository-url https://test.pypi.org/legacy/ dist/* -u daniccast -p $psstestpy '
            }
            post{
                always{
                    echo 'Completed'
                }
            }
        }
    }
}
