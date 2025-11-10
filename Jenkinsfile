pipeline {
    agent { docker { image 'python:3.9-slim' } }

    options {
        timestamps()
        ansiColor('xterm')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Construyendo el proyecto...'
                sh 'python --version'
            }
        }
        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
                sh "python - <<'PY'\nfrom app import main\nassert main()==\"OK\", 'La función main no devolvió OK'\nprint('Tests OK')\nPY"
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Instalando dependencias y herramientas de seguridad...'
                sh 'python -m pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
                echo 'Ejecutando análisis estático con Bandit...'
                sh 'bandit -r . || true'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado.'
        }
    }
}
