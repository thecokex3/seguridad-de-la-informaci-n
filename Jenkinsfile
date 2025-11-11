Groovy
pipeline {
    // Usamos un agente que tenga python3 y pip
    agent { docker { image 'python:3.9-slim' } } 

    stages {
        stage('Build') {
            steps {
                echo 'Construyendo el proyecto...'
            }
        }
        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Instalando herramientas de seguridad...'
                // Instala las dependencias y la herramienta bandit
                sh 'pip install -r requirements.txt'

                echo 'Ejecutando análisis estático con Bandit...'
                // Ejecuta bandit. 
                // '|| true' es para que el pipeline no falle si encuentra 
                // vulnerabilidades, solo queremos el reporte por ahora.
                sh 'bandit -r . || true' 
            }
        }
    }
}

