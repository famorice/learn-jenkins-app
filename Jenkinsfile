pipeline {
    agent any

    environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('Deploy to AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli:2.24.10'
                    reuseNode true
                    args '--entrypoint=""'
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json
                        aws ecs update-service --cluster LearnJenkinsApp-Cluster-Prod --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:2
                    '''
                }                
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Build stage"
                    ls -la
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        // stage('Tests') {
        //     parallel {
        //         stage('Unit tests') {
        //             agent {
        //                 docker {
        //                     image 'my-playwright'
        //                     reuseNode true
        //                 }
        //             }
        //             steps {
        //                 sh '''
        //                     echo "Unit tests stage"
        //                     test -f build/index.html
        //                     npm test
        //                 '''
        //             }
        //             post {
        //                 always {
        //                     junit 'jest-results/junit.xml'
        //                 }
        //             }
        //         }
        //         stage('Local E2E tests') {
        //             agent {
        //                 docker {
        //                     image 'my-playwright'
        //                     reuseNode true
        //                 }
        //             }
        //             steps {
        //                 // start the server in the build directory and run tests
        //                 sh '''
        //                     echo "Local E2E tests stage"
        //                     serve -s build &
        //                     sleep 10
        //                     npx playwright test
        //                 '''
        //             }
        //             post {
        //                 always {
        //                     publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright Local HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        //                 }
        //             }                    
        //         }       
        //     }
        // }
        // stage('Deploy staging') {
        //     agent {
        //         docker {
        //             image 'my-playwright'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             echo "Deploy stage"
        //             echo "Deploying to staging. Site ID: $NETLIFY_SITE_ID"
        //             netlify status
        //             netlify deploy --dir=build --json > deploy-output.json
        //         '''
        //         script {
        //             env.STAGING_URL = sh(script: 'jq -r ".deploy_url" deploy-output.json', returnStdout: true)
        //         }
        //     }
        // }
        // stage('Staging E2E tests') {
        //     agent {
        //         docker {
        //             image 'my-playwright'
        //             reuseNode true
        //         }
        //     }
        //     environment {
        //         CI_ENVIRONMENT_URL = "$env.STAGING_URL"
        //     }
        //     steps {
        //         sh '''
        //             echo "Staging E2E tests stage"
        //             echo "Staging URL is: $CI_ENVIRONMENT_URL"
        //             npx playwright test
        //         '''
        //     }
        //     post {
        //         always {
        //             publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright Staging E2E HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        //         }
        //     }                    
        // }
        /*
        stage('Approval') {
            steps {
                input message: 'Do you wish to deploy to production ?', ok: 'Yes, I am sure!'
            }
        }
        */

        // stage('Deploy prod') {
        //     agent {
        //         docker {
        //             image 'my-playwright'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             echo "Deploy stage"
        //             netlify --version
        //             echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
        //             netlify status
        //             netlify deploy --dir=build --prod
        //         '''
        //     }
        // }
        // stage('Prod E2E tests') {
        //     agent {
        //         docker {
        //             image 'my-playwright'
        //             reuseNode true
        //         }
        //     }
        //     environment {
        //         CI_ENVIRONMENT_URL = 'https://jocular-douhua-8e3be8.netlify.app'
        //     }
        //     steps {
        //         sh '''
        //             echo "Prod E2E tests stage"
        //             echo "Prod URL is: $CI_ENVIRONMENT_URL"
        //             npx playwright test
        //         '''
        //     }
        //     post {
        //         always {
        //             publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright Prod E2E HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        //         }
        //     }                    
        // }  
    }
}