pipeline {
  agent any
  options { timestamps()}

  stages {
    stage('Build') {
      steps {
        echo 'Construyendo el proyecto...'
        sh 'docker --version'  // para verificar que Jenkins ve Docker
      }
    }

    stage('Test') {
      steps {
        echo 'Ejecutando pruebas (en contenedor Python)...'
        sh '''
          docker run --rm -v "$PWD":/ws -w /ws python:3.9-slim \
            sh -c "python - <<'PY'
from app import main
assert main()== 'OK', 'La función main no devolvió OK'
print('Tests OK')
PY"
        '''
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Ejecutando Bandit (en contenedor Python)...'
        sh '''
          docker run --rm -v "$PWD":/ws -w /ws python:3.9-slim \
            sh -c "python -m pip install --upgrade pip && \
                   pip install -r requirements.txt && \
                   bandit -r . || true"
        '''
      }
    }
  }

  post {
    always { echo 'Pipeline finalizado.' }
  }
}

