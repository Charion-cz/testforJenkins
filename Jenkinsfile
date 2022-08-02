// 流水线脚本
pipeline{
    // 全部的CICD流程都需要在这里

    // 任何一个代理可用就可以执行
    agent any

    // 定义环境信息
    environment {
      key = "123456"
      user = "root"
      WS = "${WORKSPACE}"
    }

    // 定义流水线的加工流程
    stages {
        // 流水线的所有阶段
        // 1.编译
        stage('版本检查') {
            steps {
               echo "$key"
               echo "${user}"
                sh 'printenv'
                sh 'echo ${GIT_BRANCH}'
                echo "${GIT_BRANCH}"
                sh 'java -version'
                sh 'git --version'
                sh 'docker version'
                sh 'pwd && ls -alh'
            }
        }

        // 2.测试
        stage('代码编译') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /var/jenkins_home/appconfig/maven/.m2:/root/.m2'
                }
            }
            steps {
               // 当前在哪个文件夹，查看文件夹内容
               sh 'pwd && ls -alh'
               sh 'mvn -v'
               sh 'cd ${WS} && mvn clean package -s "/var/jenkins_home/appconfig/maven/settings.xml"  -Dmaven.test.skip=true '
            }
        }

        stage('代码打包') {
            steps {
                echo "打包"
                // 检查Jenkins的docker命令是否能运行
                sh 'pwd && ls -alh'
            }
        }

        stage('生成镜像'){
                    steps {
                        echo "打包..."
                        //检查Jenkins的docker命令是否能运行
                        sh 'docker version'
                        sh 'pwd && ls -alh'
                        sh 'docker build -t java-devops-demo .'

                    }
        }

        //4、部署
         stage('部署'){
                    steps {
                        echo "部署..."
                        sh 'docker rm -f java-devops-demo-dev'
                        sh 'docker run -d -p 8888:8080 --name java-devops-demo-dev java-devops-demo'
                    }
        }
    }
}