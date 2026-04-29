stage('E2E') {
            agent {
                docker {
                    // VERSIÓN CORREGIDA PARA QUE HAGA MATCH
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g serve
                    npx serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }