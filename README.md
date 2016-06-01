# springboot-aop restcontroller安全
基于rest的开放api是后端开发的趋势，为了减少对业务逻辑的侵入，通过aop拦截验证api请求是非常好的选择。
在拦截rest 请求时，可以做一些安全方面的攻防,如：
* 白名单过滤
* 行为记录（根据行为记录，可以动态的生成黑名单，如12306，您的操作频率过快）
* 安全检查，计算安全码，只有校验通过的才允许访问资源。
  本demo技术要素如下.
* springboot-security
* springboot web security config

## gradle配置
```
group 'com.njp.learn'
version '1.0-SNAPSHOT'

buildscript {
    repositories {
        maven { url "https://repo.spring.io/libs-release" }
        mavenLocal()
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.3.RELEASE")
    }
}
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'maven'
apply plugin: 'spring-boot'
jar {
    baseName = 'springbootgradle'
    version =  '0.1.0'
}
repositories {
    mavenLocal()
    mavenCentral()
    maven { url "https://repo.spring.io/libs-release" }
}

dependencies {
    compile("org.springframework.boot:spring-boot-starter")
    compile("org.springframework.boot:spring-boot-starter-web")
    compile("org.springframework.boot:spring-boot-starter-tomcat")
    compile("org.springframework.boot:spring-boot-starter-security")
    compile("org.springframework:spring-aop:4.1.5.RELEASE")
    compile("org.springframework:spring-context-support:4.1.5.RELEASE")
    testCompile("junit:junit")
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.9'
}

```

## web aop 配置
```
@Configuration
public class WebAppConfig extends WebMvcConfigurerAdapter {
    /**
     * 配置拦截器
     * @author lance
     * @param registry
     */
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserSecurityInterceptor()).addPathPatterns("/api/**");
    }
}

class UserSecurityInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        System.out.println("正在进行拦截");
        System.out.println("在这里进行安全攻防");

        /*

        // 安全检查，ip白名单，ip黑名单，访问频次过滤等
        if( isvalid(req){
            // 记录行为

            return true;
        }else{
            // 拒绝
            return false;
        }

         */

        return true;  // 放过
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}

```


## web controller配置
```
@RestController
@EnableAutoConfiguration
public class ApiController {

    @RequestMapping("/api/aop")
    public String test(){
        return "hello springboot aop";
    }
}

```

## 编辑运行
在浏览器输入
```
localhost:xxxx/api/aop
```

查看结果
