pipeline {
    agent { label 'First_Slave' }
    environment {
        NEXUS_COMMON_CREDS = credentials('0981d455-e100-4f93-9faf-151ac7e29d8a')
        NEXUS_URL = 'http://192.168.13.69:8081'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "master", url: 'https://github.com/xayukab/Testing.git', credentialsId: ""
            }
        }
        stage('Load env variables') {
            steps {
                load "${WORKSPACE}/A1_modulelist"
            }
        }
        stage('Verifying build number on Jenkins') {
            script{

            while read line
            do
              echo $line | grep -q BUILD_NUMBER
              if [ $? -eq 0  ];then
                build_number=`echo $line | awk -F"=" '{print $2}' `
                appname=`echo $line | awk -F"=" '{print $1}' | awk -F"." '{print $2}'`
                if [ appname="PIPELINE_BUILD_NUMBER"] || [ appname="GRPC_BUILD_NUMBER" ] || [ appname="TRANSACTIONBROKER_BUILD_NUMBER" ] || [ appname="COMPONENT_AGENT_BUILD_NUMBER" ];then
                  curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Appsone-2.0/build_number/ | grep -q "ERROR 404"
                  if [ $? -eq 0 ];then
                    modulename=appname
                    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com'
                  fi	
                elif [ appname="UI_BUILD_NUMBER" ]
                  curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-ui-service/build_number | grep -q "ERROR 404"
                  if [ $? -eq 0 ];then
                    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com'
                  fi
                elif [ appname="LOGFORWARDER_BUILD_NUMBER" ];then
                  curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/log-forwarder/build_number | grep -q "ERROR 404"
                  if [ $? -eq 0 ];then
                    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com'
                  fi
                elif [ appname="CONTROL_CENTER_BUILD_NUMBER" ];then
                  curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/appsone-controlcenter/65 | grep -q "ERROR 404"
                  if [ $? -eq 0 ];then
                    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com'
                  fi
                elif [ appname="JIM_BUILD_NUMBER" ];then
                  curl -u appnomic-admin:appnomic@123 http://192.168.13.39:8080/job/Java-Intrusive-Agent/81 | grep -q "ERROR 404"
                  if [ $? -eq 0 ];then
                    emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
                    Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'ayush.k@appnomic.com'
                  fi
                fi	
              fi
            done <A1_modulelist
          }
        }
    }
