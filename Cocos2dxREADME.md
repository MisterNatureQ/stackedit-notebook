cocos new -l lua -p natureqxx.game-lua.appname game-lua


mkdir game_lua_3.17_build&& cd game_lua_3.17_build
game_lua_3.17_build>cmake ../game-lua -DGEN_COCOS_PREBUILT=ON


#ndk.dir=F\:\\AndroidStudioSDK\\ndk-bundle
ndk.dir=F\:\\AndroidStudioSDK\\android-ndk-r16b

$(call import-add-path, $(LOCAL_PATH)/../../../../cocos2d-x/extensions)  
$(call import-add-path, $(LOCAL_PATH)/../../../../cocos2d-x/external)

#Android Studio 更换国内源下载依赖库

buildscript {
    repositories {
        maven {url 'http://maven.aliyun.com/nexus/content/groups/public/'}
//        mavenLocal()
//        mavenCentral()
//        google()
//        jcenter()
    }
	
	
#Android Studio解决org.gradle.api.resources.ResourceException: Could not get resource
https://blog.csdn.net/qq_36982160/article/details/80722093?utm_source=blogxgwz0


#Android Studio首次连接不上网易mumu模拟器解决办法
https://blog.csdn.net/piqtytu520/article/details/78908966?utm_source=blogxgwz2
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUxNDMyMzk5MCwtMTg5ODYyMDg3NCwtMT
A2ODA2NTc5XX0=
-->