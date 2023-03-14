pipeline {
    agent {label "Java_11_Slave"}
    stages {
        stage('Stage 1: Git Pull') {
            steps {
                echo 'GIT PULL START'
                git  branch:'main', credentialsId: 'Git_Credentials', url: 'https://github.com/abbasmirza-bfdl/basic_temp.git'
                echo 'GIT PULL END'
                echo '$WORKSPACE'
            }}
        stage("Stage 2: Creating a zip package"){
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
         stage("Stage 3: Dumping package data to EC2"){
                steps{
                    sshagent(credentials : ['EC2Key']) {
                        sh script:'''
                        ssh -o StrictHostKeyChecking=no ec2-user@44.201.7.203 '
                        sudo su
                        sudo yum update -y
                        sudo yum install httpd -y
                        sudo service httpd start
                        sudo chkconfig httpd on
                        sudo chmod o+w /var/www/html
                        '
                        scp utils.zip ec2-user@43.205.128.135:/var/www/html
                        ssh -o StrictHostKeyChecking=no ec2-user@44.201.7.203 '
                        cd /var/www/html
                        unzip utils.zip -d .
                        sudo service httpd restart
                        '
                        '''
                    }
                }
            }       
                
        }
    }

