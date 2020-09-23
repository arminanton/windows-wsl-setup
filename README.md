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
