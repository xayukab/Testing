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
                        cat A1_modulelist | grep 'BUILD_NUMBER' | while read line
                        do
                            build_number=`echo $line | awk -F"=" '{print $2}' | tr -d " "`
                            appname=`echo $line | awk -F"=" '{print $1}' | awk -F"." '{print $2}'|tr -d " "`
                            if [ $appname = "PIPELINE_BUILD_NUMBER" ] || [ $appname = "GRPC_BUILD_NUMBER" ] || [ $appname = "TRANSACTIONBROKER_BUILD_NUMBER" ] || [ $appname = "COMPONENT_AGENT_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Appsone-2.0/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result == "no" ];then
                                    appsonebuildresult=0
                                fi  
                            elif [ $appname = "UI_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-ui-service/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    uibuildresult=0
                                fi
                            elif [ $appname = "LOGFORWARDER_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/log-forwarder/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    logforwarderbuildresult=0
                                fi
                            elif [ $appname = "CONTROL_CENTER_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-controlcenter/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    controlcenterbuildresult=0
                                fi
                            elif [ $appname="JIM_BUILD_NUMBER" ];then
                                var=`curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Java-Intrusive-Agent/${build_number}/`
                                result=`echo "HTTP ERROR 404" | awk -v var="$var" '{ if ( match(var, $0) && length < length(var) ) { print "yes" } else { print "no" } }'`
                                if [ $result = "yes" ];then
                                    jimbuildresult=0
                                fi
                            fi  
                        done
                '''
                script {
                    if(env.appsonebuildresult==0){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'abhishek.d@appnomic.com'
                    }
                    if (env.uibuildresult==0){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                        Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'abhishek.d@appnomic.com'
                    }
                    if(env.logforwarderbuildresult==0) {
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'abhishek.d@appnomic.com'
                    }
                    if(env.controlcenterbuildresult==0){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'abhishek.d@appnomic.com'
                    }
                    if(env.jimbuildresult==0){
                        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                                            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'abhishek.d@appnomic.com'
                    }
                }
            }   
        }
    }  
}
