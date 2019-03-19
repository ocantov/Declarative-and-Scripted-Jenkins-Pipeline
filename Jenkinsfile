pipeline {
        agent {
        kubernetes {
        label 'jenkins-agent'
        containerTemplate {
            name 'alpine'
            image 'alpine'
            ttyEnabled true
            command 'cat'
        }
        }
    }
    stages {
        stage('Build') {
               steps {
                sh "./progress.sh"   
                stash includes: '**/*', name: 'workspace'
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                     withEnv(["service=App_A"]) {                        
                        sh 'echo Unit Tests for $service'
                        sh 'echo Success.......'
                    }
                    }
                    post {
                        always {
                            sh 'echo post unit tests results'
                        }
                    }
                }
                stage('Integration Tests') {
                    steps {
                     withEnv(["service=App_A"]) {                        
                        sh 'echo Integration Tests for $service'
                        sh 'echo Success.......'
                     }
                    }
                    post {
                        always {
                             sh 'echo post integration tests results'
                        }
                    }
                }
            }
        }

        stage ('Push packages'){
             when {
                branch "master"
                }
             steps {
                unstash 'workspace'
                sh "./progress.sh"
                script {
                    def packages = ['Docker', '.ZIP']
                    for (int i = 0; i < packages.size(); ++i) {
                        echo "Uploading the ${packages[i]} to the artifacts repository"
                    }
                }
            }
        }

    }
}