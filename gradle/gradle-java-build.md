# Gradle

Last Edited: Feb 10, 2019 1:29 AM
Tags: Gradle

# 자바 어플리케이션 빌드하고 실행하는 법

[https://guides.gradle.org/building-java-applications/](https://guides.gradle.org/building-java-applications/)

# Jar 파일 생성 후 실행 하는 법

build.gradle 파일에서 jar 관련 설정을 해주면 된다.
```
    jar {
        manifest {
    				attributes 'Main-Class': 'com.minsub.sample.jar.Main'    
    		}    
    		from {        
    				configurations.compile.collect {            
    						it.isDirectory() ? it : zipTree(it)        
    				}    
    		}
    }
```
manifest 에서 main-class를 지종하고 from 에서 dependeny에서 사용하는 library들을 jar로 묶어준다.

해당 설정으로 build를 하거나 jar를 실행하면 /build/libs 폴더에 jar 파일이 생성된다.
```
    # 7070 포트에 백그라운드로 jar 실행
    java -jar build/libs/web-application-server-1.0.jar 7070 &
```