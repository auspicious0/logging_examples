


# log 관련 예제
공통 필요 라이브러리


 ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/2ec97496-43a2-4a94-b7ac-6f55b051d5ca)

 # 목차

- [마이파티스 예제](#마이파티스-예제)
  - [공통 필요 라이브러리](#공통-필요-라이브러리)
  - [예제 1. common logging, log4j로 로그 파일 생성](#예제-1-common-logging-log4j로-로그-파일-생성)
  - [예제 2 slf4j로 로그파일 생성](#예제-2-slf4j로-로그파일-생성)
  - [예제 3 tinylog를 통해 log file 생성](#예제-3-tinylog를-통해-log-file-생성)
  - [예제 4 slf4j를 통해 log4j 로그파일 생성](#예제-4-slf4j를-통해-log4j-로그파일-생성)
    

## 예제 1. common logging, log4j로 로그 파일 생성

### 코드 

```
import org.apache.commons.logging.LogFactory;
import org.apache.log4j.Logger;
import org.apache.log4j.PropertyConfigurator;

public class HelloWorld {
    private static final Logger logger = Logger.getLogger(HelloWorld.class);

    public static void main(String[] args) {
        // Log4j 초기화
    	PropertyConfigurator.configure("conf/log4j.properties");
        
    	logger.info("HelloWorld");   
    }
}
```


### 환경 설정

```
log4j.rootLogger=info, file

log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=log/mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{HH:mm:ss} %-5p %c{1}:%L - %m%n
```

필요 라이브러리

 ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/2501fb3f-d636-47e1-8b1f-4ebd4222d88d)
![image](https://github.com/auspicious0/mabatis_example/assets/108572025/2c00116c-db98-4b14-83c0-28315ff2abf7)


       

## 예제 2 slf4j로 로그파일 생성
 
 ### 코드

 ```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import ch.qos.logback.classic.LoggerContext;
import ch.qos.logback.classic.joran.JoranConfigurator;

public class HelloWorld_slf4j {
    private static final Logger logger = LoggerFactory.getLogger(HelloWorld_slf4j.class);

    public static void main(String[] args) {
        LoggerContext context = (LoggerContext) LoggerFactory.getILoggerFactory();

        JoranConfigurator configurator = new JoranConfigurator();
        configurator.setContext(context);
        context.reset(); // Clear any existing configuration

        try {
            configurator.doConfigure("conf/logback.xml"); // Load Logback configuration
        } catch (Exception e) {
            e.printStackTrace();
        }

        logger.info("HelloWorld");
    }
}
```

### 환경설정

```
<configuration>
    <!-- 로그 출력 포맷 지정 -->
    <property name="LOG_PATTERN" value="%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{35} - %msg%n" />

    <!-- 기본 로그 레벨 설정 (TRACE, DEBUG, INFO, WARN, ERROR) -->
    <root level="info">
        <appender-ref ref="FILE" />
    </root>

    <!-- 파일 로깅 설정 -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <!-- 로그 파일 경로 -->
        <file>log/slf4j.log</file>
        <encoder>
            <pattern>${LOG_PATTERN}</pattern>
        </encoder>
    </appender>
</configuration>
```

### 필요 라이브러리

    
 
  ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/11506f28-154b-4577-b48c-51cf7333904a)
  ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/0475bc1a-b29d-44a0-82ce-34819aec67c5)
  ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/99f37cb3-2d62-419a-ba17-f823cdd905db)





## 예제 3 tinylog를 통해 log file 생성

### 코드 

```
import org.pmw.tinylog.Configurator;
import org.pmw.tinylog.Logger;

import java.io.File;
import java.io.IOException; // IOException을 import 추가

public class HelloWorld_slf4j_tinylog2 {
    public static void main(String[] args) {
        File configFile = new File("conf/tinylog.properties");

        try {
            // 설정 파일의 경로 지정
            Configurator.fromFile(configFile).activate();
        } catch (IOException e) {
            System.err.println("Failed to load configuration file: " + e.getMessage());
            e.printStackTrace();
            return;
        }

        // 로그 메시지 출력
        Logger.info("HelloWorld");
    }
}
```

### 환경설정

```
tinylog.level = info
tinylog.writer = file
tinylog.writer.filename = log/tinylog.log
```

### 필요 라이브러리

 ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/339943eb-57c1-4c36-87ce-cb6cb95df2b3)


## 예제 4 slf4j를 통해 log4j 로그파일 생성

### 코드

```
import org.apache.log4j.PropertyConfigurator;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
	 private static final Logger logger = LoggerFactory.getLogger(HelloWorld.class);

    public static void main(String[] args) {
        // Log4j 초기화
    	PropertyConfigurator.configure("conf/log4j.properties");
        
    	logger.info("HelloWorld");   
    }
}
```


### 환경설정 – 동일

필요 라이브러리 

 ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/71579110-c0d8-4fb1-a5d9-f15097547a0e)
 ![image](https://github.com/auspicious0/mabatis_example/assets/108572025/6aae7f19-71cd-421e-afbf-15be96ee0324)

### 예제 4번에 대한 해석
slf4j는 로깅 시스템을 위한 추상 계층이다. 

slf4j를 사용하면 코드에서 로깅 시스템에 대한 종속성을 제거할 수 있다. 

slf4j를 통해 로깅 이벤트를 전달하고 실제 로깅 시스템은 log4j에서 벡엔드로서 설정될 수 있기 때문에 로그파일이 생성되는 것이다. 

#### log4j-over-slf4 : 전달체

slf4j는 자체적으로 로그 메시지를 처리하지 않는다. 

대신 slf4j api를 구현하는 다른 로깅 프레임워크에 로그 이벤트를 전달한다. 

이 때 사용되는 것이 바로 slf4j 브릿지이다. 

log4j-over-slf4j 라이브러리는 slf4j api를 log4j에 연결해주는 역할을 한다. 

따라서 slf4j를 사용하여 로그를 작성하면 slf4j는 log4j-over-slf4j를 통해 log4j에게 로그 이벤트를 전달 할 수 있게 되는 것이다.

#### log4j-slf4j-impl : 구현체

slf4j는 자체적으로 로깅 이벤트를 처리하지 않기 떄문에 slf4j api를 구현하는 로깅 프레임워크가 필요하다.

log4j-sl4j-impl 라이브러리는 slf4j api를 구현한 log4j의 구현체이다.

이 라이브러리는 slf4j를 사용하는 애플리케이션에서 log4j를 로깅 시스템으로 사용할 수 있도록 해준다. 

 
