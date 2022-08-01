// 流水线脚本
pipeline{
    // 全部的CICD流程都需要在这里

    // 任何一个代理可用就可以执行
    agent any

    // 定义环境信息
    environment {
      key = "123456"
      user = "root"
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
               echo "打包"
               // 当前在哪个文件夹，查看文件夹内容
               sh 'pwd && ls -alh'
               sh 'mvn -v'
               // 打包
               // sh 'mvn clean package -Dmaven.test.skip=true -s "/var/jenkins_home/appconfig/maven/settings.xml" '
            }
        }

        stage('代码打包') {
            steps {
                echo "打包"
                // 检查Jenkins的docker命令是否能运行
                sh 'docker version'
            }
        }
    }
}