# Running Multiple Spring Boot JARs on the same Windows App Service

- Create two Virtual Applications:

![image](https://github.com/gkgaurav31/multiple-jar-windows-webapp/assets/9574170/283a8120-5629-4871-a1e6-d7f67a4a19e0)

- Create two folders corresponding to the above Virtual Applications. Upload the app.jar and web.config files. Refer to the sample shared to check the folder structure.

The ```web.config``` has the main logic to start the two applications:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
 <system.webServer>
  <handlers>
    <add name="httpPlatformHandler" path="*" verb="*"   modules="httpPlatformHandler" resourceType="Unspecified" />
  </handlers>
  <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Dserver.servlet.context-path=/app1 -Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\app1\app.jar&quot;"
      stdoutLogEnabled="true" 
      startupRetryCount='10'>
  </httpPlatform>
 </system.webServer>
</configuration>
```

Note that, if you select Java SE as the stack, the platform will start another default app. You could switch the stack to ASP.NET and the things should still work. However, it will default to Java 8. If the app is not compatible with Java 8, it will fail. As a work-around, change the stack to Tomcat with Java 17 or Java 17 with Embedded Tomcat.

Also note that, there will be a JVM for each of the above Spring Boot App. It may be more convenient to create different WebApps for each Java Application.
