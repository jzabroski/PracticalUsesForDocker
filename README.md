# PracticalUsesForDocker
For Docker talk at NEMD December 2019

# Getting Started Installing Docker on Windows

We'll be using PowerShell Core, Docker Desktop and Docker Kinematic.

These tools take time to install, as does pulling down your first docker images.

## Pre-requisites

Docker Desktop requires **Windows 10 Pro**.
If you try to install Docker Desktop on Windows 10 Home, it will fail.

## Install Chocolatey, PowerShell Core, Docker Desktop and Docker Kinematic

1. Open PowerShell Classic, using Run As Administrator, and run the following command:
    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
    ```

2. Close the PowerShell Classic window.
3. Open a new PowerShell Classic window, using Run As Administrator, and run the following command:
    ```powershell
    choco install powershell-core -y
    ```
4. Close PowerShell Classic window.
5. Open PowerShell Core window, using Run As Administrator, and run the following commands:
    ```powershell
    choco install docker-desktop -y
    choco install docker-kitematic -y
    ```

6. After these install, you have to log out of Windows and log back in, so that your user account's `docker-users` group membership takes effect.

7. After logging back in, open Windows PowerShell Core and run the following command:
    ```powershell
    run docker run -d -p 1433:1433 -p 80:80 -v C:/ssrs/:C:/sqldata/ -e sa_password=Compl3xPassw0rd -e ssrs_user=SSRSAdmin -e ssrs_password=Compl3xPassw0rd4Ssrs randreng/ssrs --attach
    ```
    This command does a couple of things, which you can look in the [commandline/run](https://docs.docker.com/engine/reference/commandline/run/) documentation:
    1. `-d` : Run container in background and print container ID
    2. `-p 1433:1433` : Publish a container’s port (1433) to the host (1433)
    3. `-p 80:80` : Publish a container's port (80) to the host (80)
    4. `-v C:/ssrs/:C:/sqlData/` : [Bind mount](https://docs.docker.com/engine/reference/commandline/run/#mount-volume--v---read-only) a volume.  In this case, we're mounting C:/ssrs/ to C:/sqlData/
        a. When the host directory of a bind-mounted volume doesn’t exist, Docker will automatically create this directory on the host for you. In the example above, Docker will create the /doesnt/exist folder before starting your container.
        b. On Windows, the paths must be specified using Windows-style semantics.


    3. `
