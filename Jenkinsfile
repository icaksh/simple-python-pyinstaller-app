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
    stage('Manual Approval'){
        input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
    }
    stage('Deploy') {
        withDockerContainer(args: "--entrypoint=''", image: 'six8/pyinstaller-alpine-linux-amd64:alpine-3.12-python-2.7-pyinstaller-v3.4'){
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