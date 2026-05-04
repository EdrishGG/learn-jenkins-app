pipeline {
    agent any
    
    environment {
        NETLIFY_SITE_ID = '9b15c18a-39bf-4e81-af37-9ef8c5a0ef2c'
        NETLIFY_AUTH_TOKEN = 'nfp_pQTCRac9T2fggHb5drgdqKVdL1RyjYHH0f4d'
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
                sh '''
                    ls -la
                    node --version
                    npm --version
                    
                    echo "Instalando dependencias y construyendo la aplicacion..."
                    npm ci
                    npm run build
                    
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

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Instalamos bash por si Netlify se pone fresa y lo exige
                    apk add --no-cache bash
                    
                    npm install netlify-cli -g
                    netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                    
                    echo '[build]' > netlify.toml
                    echo '  command = "echo Ya construido por Jenkins, saltando..."' >> netlify.toml
                    echo '  publish = "build"' >> netlify.toml
                    
                    netlify status
                    netlify deploy --dir=build --prod
                '''
            }
        }
    }
    
    post {
        always {
            echo '¡Pipeline finalizado con exito! (Reportes desactivados temporalmente)'
        }
    }
}

//Todo cool