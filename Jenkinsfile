pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                echo 'Clonando repositorio...'
                checkout scm
            }
        }

        stage('Verificar cambios') {
            steps {
                bat 'git fetch'
                script {
                    def cambios = bat(script: "git diff --name-only origin/main", returnStdout: true).trim()
                    if (cambios) {
                        echo "⚠ Se detectaron cambios respecto a origin/main:"
                        echo cambios
                    } else {
                        echo "✅ No hay cambios respecto a origin/main."
                    }
                }
            }
        }

       stage('Análisis de Seguridad con Bandit') {
            steps {
                script {
                    def status = bat(script: 'bandit -r . > reporte_bandit.txt', returnStatus: true)
                    bat 'type reporte_bandit.txt'
                    if (status != 0) {
                        echo "⚠ Bandit encontró posibles vulnerabilidades. Revisa el reporte."
                        // Puedes marcar aquí el build como inestable si quieres:
                        // currentBuild.result = 'UNSTABLE'
                    } else {
                        echo "✅ Análisis de Bandit finalizado sin problemas."
                    }
                }
            }
        }



        stage('Construir') {
            steps {
                echo 'Construyendo el proyecto......'
            }
        }

        stage('Pruebas') {
            steps {
                echo 'Ejecutando pruebas......'
            }
        }

        stage('Despliegue') {
            steps {
                echo 'Desplegando en entorno de pruebas......'
            }
        }
    }
}
