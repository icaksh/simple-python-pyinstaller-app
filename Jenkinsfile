node {
    stage('Build') {
        withDockerContainer(image: 'python:2-alpine'){
            sh 'python -m py_compile $(pwd)/sources/add2vals.py $(pwd)/sources/calc.py' 
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    try{
        stage('Test') {
            withDockerContainer(image: 'qnib/pytest'){
                sh 'py.test --junit-xml $(pwd)/test-reports/results.xml $(pwd)/sources/test_calc.py'
            }
        }
    } catch (e) {
        throw e
    } finally {
        junit '$(pwd)/test-reports/results.xml'
    }
}