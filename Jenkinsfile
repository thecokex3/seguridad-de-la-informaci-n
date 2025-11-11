pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Build') {
      steps {
        echo 'Construyendo el proyecto...'
        sh 'docker --version'
        sh 'ls -la'                // Ver archivos en el workspace de Jenkins
      }
    }

    stage('Test') {
      steps {
        echo 'Ejecutando pruebas (en contenedor Python)...'
        sh '''
          docker run --rm -v "$PWD":/ws -w /ws python:3.9-slim \
            sh -lc "ls -la; python -c \\"from app import main; assert main()== 'OK', 'La función main no devolvió OK'; print('Tests OK')\\""
        '''
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Ejecutando Bandit (en contenedor Python)...'
        sh '''
          docker run --rm -v "$PWD":/ws -w /ws python:3.9-slim \
            sh -lc "python -m pip install --upgrade pip && pip install -r requirements.txt && bandit -r . || true"
        '''
      }
    }
  }

  post {
    always { echo 'Pipeline finalizado.' }
  }
}
