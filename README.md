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
