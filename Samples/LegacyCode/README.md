# IIS
```dockerfile
FROM microsoft/windowsservercore

RUN PowerShell -Command Add-WindowsFeature Web-Server

ADD ServerMonitor.exe /ServiceMonitor.exe

EXPOSE 80

ENTRYPOINT ["C:\\ServiceMonitor.exe", "w3svc"]
```

# ASP.NET 3.5 Dockerfile 
```dockerfile
FROM microsoft/iis

ADD install /build

RUN DISM /Online /Add-Package /PackagePath:C:\build\microsoft-windows-netfx3-ondemand-package.cab & \
    DISM /Online /Add-Package /PackagePath:C:\build\patch\Windows10.0-KB3213986-x64.cab & \
    rd /s /q C:\build & \
    PowerShell-Command Add-WindowsFeature NET-Framework-45-ASPNET; & \
    %WINDIR%\SysWOW64\inetsrv\appcmd set app "Default Web Site/" /applicationPool:".NET v2.0" & \
    PowerShell -Command Remove-Item -Recurse C:\inetpub\wwwroot\*
```

# ASP.NET 4.6.2 Dockerfile 

```dockerfile
FROM microsoft/iis

RUN PowerShell -Command Add-WindowsFeature NET-Framework-45-ASPNET; \
    PowerShell -Command Add-WindowsFeature Web-Asp-Net45; \
    PowerShell -Command Remove-Item -Recurse C:\inetpub\wwwroot\*
    
```