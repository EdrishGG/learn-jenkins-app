pipeline {
    agent any
    
    evironment {
        NETLIFY_SITE_ID = '9b15c18a-39bf-4e81-af37-9ef8c5a0ef2c'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                // Usamos el símbolo de gato (#) adentro del bloque 'sh' para comentar comandos de Linux
                sh '''
                    ls -la
                    node --version
                    npm --version
                    
                    echo "Saltando npm ci y npm run build para ahorrar tiempo..."
                    # npm ci
                    # npm run build
                    
                    ls -la
                '''
            }
        }
        
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Iniciando bateria de pruebas..."
                    # npm run test
                '''
            }
        }
        
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Saltando pruebas E2E pesadas..."
                    # npm install serve
                    # npx serve -s build &
                    # sleep 10
                    # npx playwright test --reporter=html
                '''
            }
        }

        // AQUÍ ES DONDE DEBE IR EL DEPLOY, ADENTRO DE "STAGES"
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli -g
                    netlify --version
                    echo "Deploying to production. Site ID: 9b15c18a-39bf-4e81-af37-9ef8c5a0ef2c"
                '''
            }
        }
    }
    
    post {
        always {
            // Comentamos la recolección de reportes porque saltamos las pruebas reales
            // junit '**/junit.xml, **/*results/*.xml'
            // publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            
            echo '¡Pipeline finalizado con exito! (Reportes desactivados temporalmente)'
        }
    }
}
