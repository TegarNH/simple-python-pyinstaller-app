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
    withDockerContainer('six8/pyinstaller-alpine-linux-amd64:alpine-3.12-python-2.7-pyinstaller-v3.4') {
        stage('Deploy') {
          sh 'pyinstaller --onefile sources/add2vals.py'
          archiveArtifacts artifacts: 'dist/add2vals'
        }
    }
}

