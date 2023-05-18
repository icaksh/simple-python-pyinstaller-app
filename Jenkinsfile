node {
    stage('Build') {
        withDockerContainer(image: 'python:2-alpine'){
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    stage('Test') {
        try{
            withDockerContainer(image: 'qnib/pytest'){
                checkout scm
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } catch (e) {
            throw e
        } finally {
            checkout scm
            junit 'test-reports/results.xml'
        }
    }

    stage('Deploy') {
        try{
            withDockerContainer(image: 'cdrx/pyinstaller-linux:python2'){
                checkout scm
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
        } catch (e) {
            throw e
        } finally {
            checkout scm
            archiveArtifacts 'dist/add2vals'
            sleep 60
        }
    }
}