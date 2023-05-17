node {
    stage('Build') {
        withDockerContainer(args: '-v $(pwd)/sources:/sources',image: 'python:2-alpine'){
            sh 'python -m py_compile /sources/add2vals.py /sources/calc.py' 
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    try{
        stage('Test') {
            withDockerContainer(image: 'qnib/pytest'){
                sh 'py.test --junit-xml /test-reports/results.xml /sources/test_calc.py'
            }
        }
    } catch (e) {
        throw e
    } finally {
        junit '/test-reports/results.xml'
    }
}