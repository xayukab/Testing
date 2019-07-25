pipeline {
    agent { label 'First_Slave' }
   
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "master", url: 'https://github.com/xayukab/Testing.git', credentialsId: ""
            }
        }
     
        stage('Verifying build number on Jenkins') {
            steps {
                sh '''
			> result
                        cat A1_modulelist | grep 'BUILD_NUMBER' | while read line
                        do
                            build_number=`echo $line | awk -F"=" '{print $2}' | tr -d " "`
                            appname=`echo $line | awk -F"=" '{print $1}' | awk -F"." '{print $2}'|tr -d " "`
                            if [ $appname = "PIPELINE_BUILD_NUMBER" ] || [ $appname = "GRPC_BUILD_NUMBER" ] || [ $appname = "TRANSACTIONBROKER_BUILD_NUMBER" ] || [ $appname = "COMPONENT_AGENT_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Appsone-2.0/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result == "yes" ];then
                                   echo "env.appsonebuildresult=0" >> result
				else
				   echo "env.appsonebuildresult=1" >> result
                                fi
                            elif [ $appname = "UI_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-ui-service/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                   echo "env.uibuildresult=0" >> result
                                fi
                            elif [ $appname = "LOGFORWARDER_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/log-forwarder/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    echo "env.logforwarderbuildresult=0" >> result
				else
				    echo "env.logforwarderbuildresult=1" >> result
                                fi
                            elif [ $appname = "CONTROL_CENTER_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-controlcenter/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    echo "env.controlcenterbuildresult=0" >> result
				else
				    echo "env.controlcenterbuildresult=1" >> result
                                fi
                            elif [ $appname="JIM_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Java-Intrusive-Agent/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                   echo "env.jimbuildresult=0" >> result
				else
				   echo "env.jimbuildresult=1" >> result
                                fi
                            fi
                        done
                '''
		load "result"
                script {
			
			echo sh(script: 'env|sort', returnStdout: true)
			if (env.appsonebuildresult=="0"){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com,krishna.p@appnomic.com'
                    }
			if (env.uibuildresult=="0"){
			echo "UI BUILD"
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                        Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com,krishna.p@appnomic.com'
                    }
			if (env.logforwarderbuildresult=="0") {
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com,krishna.p@appnomic.com'
                    }
			if (env.controlcenterbuildresult=="0"){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com,krishna.p@appnomic.com'
                    }
			if (env.jimbuildresult=="0"){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com,krishna.p@appnomic.com'
                    }
                }
            }  
        }
    }
}
