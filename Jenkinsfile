pipeline {
    agent none
    options{
        skipDefaultCheckout(true)
        timeout(time: 1, unit: 'HOURS') 
    }
    parameters {
        choice(
            choices: ['ANDROID' , 'IOS'],
            description: 'Please choose a option to build SDK',
            name: 'REQUESTED_ACTION'
        )
    }
    stages{
        stage("Android Build"){
            when {
                expression { params.REQUESTED_ACTION == 'ANDROID' }
            }
            agent { label 'xamarin-android'}
            steps{
               
                checkout scm
               
                sh'''
                    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
                    cd ${WORKSPACE} && pwd || true

                    msbuild -m HelloWorld.sln /p:AndroidBuildApplicationPackage=true /p:AndroidSdkDirectory=/usr/lib/android-sdk /p:Configuration="Release" /p:Platform="Any CPU" /restore || true

                    msbuild -m HelloWorld/HelloWorld.Android/HelloWorld.Android.csproj /p:AndroidBuildApplicationPackage=true /p:AndroidSdkDirectory=/usr/lib/android-sdk /p:Configuration="Release" /p:Platform="Any CPU" /t:PackageForAndroid /p:OutputPath="../../publish_android/" || true

                    msbuild -m HelloWorld/HelloWorld.Android/HelloWorld.Android.csproj /p:AndroidSdkDirectory=/usr/lib/android-sdk /p:Configuration="Release" /p:Platform="Any CPU" /t:SignAndroidPackage /p:OutputPath="../../publish_android/" || true

                '''

                //Pre-requisites for temporary
                sh("mkdir -p ~/.m2 && cp maven-settings.xml ~/.m2/settings.xml && chmod 755 ~/.m2/settings.xml || true")

                //Upload APK to Nexus
                withCredentials([usernamePassword(credentialsId: 'Nexus', passwordVariable: 'NXSPWD', usernameVariable: 'NXSUSR')]){
                    script{
                        sh("cd ${WORKSPACE}/publish_android && pwd &&  mvn deploy:deploy-file -Dfile=com.companyname.helloworld.apk -DgroupId=com.mlx.android -DartifactId=helloworld -Dversion=${BUILD_NUMBER}.0.0 -Dpackaging=apk -DgeneratePom=true -DrepositoryId=xamarin-releases -Durl=https://nexus.tools.rukmanand.com/repository/xamarin-releases/ -Drepo.id=xamarin-releases -Drepo.login=${NXSUSR} -Drepo.pwd=${NXSPWD}")
                    }
                }

            }
        }
        stage("IOS Build"){
            when {
                expression { params.REQUESTED_ACTION == 'IOS' }
            }
            agent { label 'xamarin-ios'}
            steps{
                checkout scm
                echo "IOS build steps goes here"
                sh '''
                    msbuild my-ios-app.csproj /p:BuildIpa=True /p:IpaPackageDir=path/to/output /p:IpaPackageName=my-ios-app-name.ipa /p:Platform=iPhone /p:Configuration=Release /t:Buil
                '''
            }
        }
    }
    post{
        failure{
            mail to: 'rukmanand@mailinator.com',
            subject: "Failed Pipeline",
            body: "Something is wrong with this build"
        }
        success{
            mail to: 'rukmanand@mailinator.com',
            subject: "Failed Pipeline",
            body: "Something is wrong with this build"
        }
    }
}