pipeline{
    //agent any  //全局必须带有agent,表明此pipeline执行节点
    agent { label 'jenkins-slave2' }
    stages{
        stage("代码clone"){
            //#agent { label 'master' }  //具体执行的步骤节点，非必须
            steps{
                sh "cd /var/lib/jenkins/workspace/pipline-test2 && rm -rf ./*"
                git credentialsId: 'e2fda943-06c1-46b1-bf4b-af2713a26281', url: 'git@gitee.com:wang-chuanhang/pipeline.git'
                echo "代码 clone完成"
            }
        }
        
        stage("代码构建"){
			steps{
				sh  "cd /var/lib/jenkins/workspace/pipline-test2/ && tar czvf myapp.tar.gz ./index.html"
			}
		}
		
	   stage("停止服务"){
			steps{
                sh  'ssh root@10.0.0.202 "/etc/init.d/tomcat stop && rm -rf  /data/tomcat/tomcat_webapps/myapp/index.html"' 
                sh  'ssh root@10.0.0.203 "/etc/init.d/tomcat stop && rm -rf  /data/tomcat/tomcat_webapps/myapp/index.html"' 
			}
		}
        
		stage("代码copy"){
			steps{
                sh 'scp /var/lib/jenkins/workspace/pipline-test2/myapp.tar.gz root@10.0.0.202:/data/tomcat/tomcat_appdir'
                sh 'scp /var/lib/jenkins/workspace/pipline-test2/myapp.tar.gz root@10.0.0.203:/data/tomcat/tomcat_appdir'
			}
		}
		
		stage("代码部署"){
			steps{
                sh 'ssh root@10.0.0.202 "tar xvf /data/tomcat/tomcat_appdir/myapp.tar.gz -C /data/tomcat/tomcat_webdir/myapp"'
                sh 'ssh root@10.0.0.203 "tar xvf /data/tomcat/tomcat_appdir/myapp.tar.gz -C /data/tomcat/tomcat_webdir/myapp"'
			}
		}
		
		
		stage("启动服务"){
			steps{
                sh  'ssh root@10.0.0.202 "/etc/init.d/tomcat start"' 
                sh  'ssh root@10.0.0.203 "/etc/init.d/tomcat start"' 
			}
		}
    }
}
