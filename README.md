# Practical Uses For Docker
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

7. After logging back in, in the Windows "Type here to search" box, search for **Docker Desktop**.
    1. As Docker starts, you'll see an event toaster in the bottom right saying docker might take a few moments to start.
    2. As its starting, it will detect you do not have Hyper-V and Container features enabled.
    3. Click OK and it will restart your computer.
    
![Docker Is So Needy](docker.png)
    
8. After your computer restarts, log back in.

9. Open Windows PowerShell Core and run the following command, which will benchmark how long it takes to pull down the image:
    ```powershell
    Measure-Command { docker pull randreng/ssrs | Out-Default }
    ```
    **This command will fail the first time you run it.  The failure message is not in red blood, so read carefully!**
    
    1. The error message is "_image operating system "windows" cannot be used on this platform_"
        1. This error message is easy to fix.  The default Docker Desktop is configured to run Linux Containers.
        2. Go to your System Tray, find the little tiny Moby whale, right click, select "Switch to Windows containers"
            1. For command line lovers, run:
                ```powershell
                & $Env:ProgramFiles\Docker\Docker\DockerCli.exe -SwitchDaemon
                ```
                As of this writing, running this twice will revert the switch.  May change in the future if there are more OS container sub-systems supported!
    2. Knowing how long it takes to download your base image will help give you a sense of how long to expect it to take to spin up a container from scratch on a new machine.
    3. This download time might _also_ convince you to invest in a local Docker Registry or a Registry co-located with your applications.
    
10. Once your image has been pulled (downloaded) from the docker registry,
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
    5. `-e sa_password=Compl3xPassw0rd` : 	Set environment variable `sa_password` to `Compl3xPassw0rd`.
        a. Note that since this is a SQL Server sa password, your machine's password policy applies.  If the password is not complex enough, you'll (probably) get a warning, although whether that warning is actually surfaced to stdout/stderr depends on who wrote the docker image you're using and how thoughtful they were :)
        b. You can see 
    6. `-e ssrs_user=SSRSAdmin` : Set environment variable `ssrs_user` to `SSRSAdmin`
    7. `-e ssrs_password=Compl3xPassw0rd4Ssrs` : Set environment variable `ssrs_password` to `Compl3xPassw0rd4Ssrs`
    8. `randreng/ssrs` : The image we wish to run.
        a. If it's not already been pulled down, docker will pull it prior to running it.
    9. `--attach` : Attaches the container to stdin, stderr, stdout
        a. Useful for troubleshooting why your container dies within the first seconds or minutes after it starts.
        b. Remember, containers have no memory, so if you don't think through how to monitor them, you won't know why they stop.
