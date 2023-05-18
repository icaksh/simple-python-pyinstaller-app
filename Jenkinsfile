node {
    stage('Build') {
        withDockerContainer(image: 'python:2-alpine'){
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    stage('Test') {
        withDockerContainer(image: 'qnib/pytest'){
            try{
                checkout scm
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (e) {
                throw e
            } finally {
                checkout scm
                junit 'test-reports/results.xml'
            }
        }
    }

    stage('Deploy') {
        withDockerContainer(image: 'cdrx/pyinstaller-linux:python2'){
            try{
                checkout scm
                sh 'pyinstaller --onefile sources/add2vals.py'
            } catch (e) {
                throw e
            } finally {
                checkout scm
                archiveArtifacts 'dist/add2vals'
                sleep 60
            }    
        }
    }
}