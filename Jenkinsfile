pipeline {
    agent any
    tools {
        nodejs 'NODEJS'
    }
    environment {
        ENVIRONMENT = 'local'
        GITHUB_TOKEN_CREDENTIAL_ID = 'your-github-token-id'
        FRONTEND_REPO_NAME = 'your-frontend-repo'
        FRONTEND_GITHUB_BRANCH = 'your-frontend-branch'
        FRONTEND_GITHUB_REMOTE = 'origin'
        BACKEND_REPO_NAME = 'your-backend-repo'
        BACKEND_GITHUB_BRANCH = 'your-backend-branch'
        BACKEND_GITHUB_REMOTE = 'origin'
        FRONTEND_PROJECT_PATH = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\your-workspace-folder"
        BACKEND_PROJECT_PATH = "${FRONTEND_PROJECT_PATH}\\your-backend-folder"
        WEBAPP_PATH = "${BACKEND_PROJECT_PATH}\\src\\main\\webapp"
        RESOURCES_PATH = "${BACKEND_PROJECT_PATH}\\src\\main\\resources"
        NEXUS_URL = "http://your-nexus-server/repository/your-nexus-repo"
        NEXUS_CREDENTIALS_ID = 'your-nexus-credentials-id'
    }

    stages {
        stage('Git Pull Frontend') {
            steps {
                script {
                    withCredentials([string(credentialsId: env.GITHUB_TOKEN_CREDENTIAL_ID, variable: 'GITHUB_TOKEN')]) {
                        git branch: env.FRONTEND_GITHUB_BRANCH, url: "https://${GITHUB_TOKEN}@github.com/your-org/${env.FRONTEND_REPO_NAME}"
                        bat "git pull ${env.FRONTEND_GITHUB_REMOTE} ${env.FRONTEND_GITHUB_BRANCH}"
                    }
                }
            }
        }

        /*
        stage('npm install') {
            steps {
                bat 'npm install --force'
            }
        }
        */

        stage('Build Frontend') {
            steps {
                script {
                    bat "ng build"
                }
            }
        }

        stage('Git Pull Backend') {
            steps {
                script {
                    withCredentials([string(credentialsId: env.GITHUB_TOKEN_CREDENTIAL_ID, variable: 'GITHUB_TOKEN')]) {
                        git branch: env.BACKEND_GITHUB_BRANCH, url: "https://${GITHUB_TOKEN}@github.com/your-org/${env.BACKEND_REPO_NAME}"
                        bat "git pull ${env.BACKEND_GITHUB_REMOTE} ${env.BACKEND_GITHUB_BRANCH}"
                    }
                }
            }
        }

        stage('Update Web Files') {
            steps {
                script {
                    bat "xcopy /s /e /y ${FRONTEND_PROJECT_PATH}\\dist\\* ${WEBAPP_PATH}\\"
                }
            }
        }

        stage('Build and Deploy WAR') {
            steps {
                dir("${BACKEND_PROJECT_PATH}") {
                    script {
                        bat "mvn clean package"

                        def date = new Date().format('yyyyMMdd.HHmmss')
                        def newWarName = "your-app-name-${ENVIRONMENT}-${date}.war"
                        bat "rename target\\*.war ${newWarName}"

                        withCredentials([usernamePassword(credentialsId: env.NEXUS_CREDENTIALS_ID, usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                            bat """
                                mvn deploy:deploy-file ^
                                -Durl=${env.NEXUS_URL} ^
                                -DrepositoryId=your-repository-id ^
                                -Dfile=target\\${newWarName} ^
                                -DpomFile=${BACKEND_PROJECT_PATH}\\pom.xml ^
                                -DgroupId=com.example ^
                                -DartifactId=your-artifact-id ^
                                -Dversion=0.0.1-SNAPSHOT ^
                                -Dpackaging=war ^
                                -DgeneratePom=false ^
                                -Dusername=%NEXUS_USERNAME% ^
                                -Dpassword=%NEXUS_PASSWORD%
                            """
                        }
                    }
                }
            }
        }
    }
}
