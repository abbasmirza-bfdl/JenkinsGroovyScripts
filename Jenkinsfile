pipeline {
    agent {label "Java_11_Slave"}
    stages {
        stage('Stage 1') {
            steps {
                echo 'GIT PULL START'
                git  branch:'main', credentialsId: 'Git_Credentials', url: 'https://github.com/abbasmirza-bfdl/basic_temp.git'
                echo 'GIT PULL END'
                echo '$WORKSPACE'
            }}
        stage("package"){
            steps{
                 script {
                        if (!fileExists('utils.zip')) {
                            sh script:'''
                            echo "ZIP START"
                            zip -r ../utils.zip *
                            echo "ZIP END"
                            '''
                            }
                        } 
                    }
                }
                
                
        }
    }

