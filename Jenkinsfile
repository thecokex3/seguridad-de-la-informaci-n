pipeline {
  agent any
  options { timestamps() }

  environment {
    JWS = "/var/jenkins_home/workspace/${JOB_NAME}"
  }

  stages {
    stage('Build') {
      steps {
        echo 'Construyendo el proyecto...'
        sh 'docker --version'
        sh 'ls -la'  // Verifica que app.py y requirements.txt estén en el workspace del Jenkins (contenedor)
      }
    }

    stage('Test') {
      steps {
        echo 'Ejecutando pruebas (en contenedor Python)...'
        sh '''
          docker run --rm \
            -v jenkins_home:/var/jenkins_home \
            -w "/var/jenkins_home/workspace/$JOB_NAME" \
            python:3.9-slim \
            python -c 'from app import main; assert main()=="OK", "La función main no devolvió OK"; print("Tests OK")'
        '''
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Ejecutando Bandit (en contenedor Python)...'
        sh '''
          docker run --rm \
            -v jenkins_home:/var/jenkins_home \
            -w "/var/jenkins_home/workspace/$JOB_NAME" \
            python:3.9-slim \
            sh -lc "python -m pip install --upgrade pip && pip install -r requirements.txt && bandit -r . || true"
        '''
      }
    }
  }

  post {
    always { echo 'Pipeline finalizado.' }
  }
}
