node {
    withDockerContainer('python:2-alpine') {
        stage('Build') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    withDockerContainer('qnib/pytest') {
        stage('Test') {
            checkout scm
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    stage('Manual Approval') {
        input 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
    }
    withDockerContainer('cdrx/pyinstaller-linux:python2') {
        stage('Deploy') {
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts 'dist/add2vals'
            sleep(60)
        }
    }
}

