# windows-wsl-setup
Notes and scripts to setup windows dev machine to use WSL

First check most recent ubuntu available for wsl.

#2020-09-23 latest LTS is 20.04.1

Make sure to enable Windows Subsystem for Linux using elevated x64 Powershell:
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

Install all the windows update available before proceeding. A couple of reboots will happen.

Enable virtualization:
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

Install WSL Kernel Update:
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

Reboot. Check if your bios has Virtualization enabled.

Run the following on elevated powershell and make sure it accepts:
wsl --set-default-version 2

Check which version of the distro that you wish to install:
https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distros

#2020-09-23 going with https://aka.ms/wslubuntu2004

Create a folder in the location you want to have your distro installed (it can be a custom drive), then run:
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing

Extract the downloaded file:
Rename-Item .\Ubuntu.appx Ubuntu.zip
Expand-Archive .\Ubuntu.zip -Verbose

Move the files to wherever you want it to be located. Then, run the EXE file containing ubuntu in the name and create your user and password.
In my case: ubuntu2004.exe

Now, let's setup zsh oh-my-zsh, alternative terminals for windows..

First, install Chocolatey has it will be usefull in the future:  (https://chocolatey.org/install)
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

Using elevated powershell and choco, let's install Fluent Terminal: (include --force if you run into problems)
choco install fluent-terminal
