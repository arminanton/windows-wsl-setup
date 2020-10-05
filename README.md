# windows-wsl-setup
Notes and scripts to setup windows dev machine to use WSL

First check most recent ubuntu available for wsl.

#2020-09-23 latest LTS is 20.04.1

Make sure to enable Windows Subsystem for Linux using elevated x64 Powershell:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Install all the windows update available before proceeding. A couple of reboots will happen.

Enable virtualization:
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Install WSL Kernel Update:
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

Reboot. Check if your bios has Virtualization enabled.

Run the following on elevated powershell and make sure it accepts:
```powershell
wsl --set-default-version 2
```

Check which version of the distro that you wish to install:
https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distros

#2020-09-23 going with https://aka.ms/wslubuntu2004

Create a folder in the location you want to have your distro installed (it can be a custom drive), then run:
```powershell
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing
```

Extract the downloaded file:
```powershell
Rename-Item .\Ubuntu.appx Ubuntu.zip
Expand-Archive .\Ubuntu.zip -Verbose
```

Move the files to wherever you want it to be located. Then, run the EXE file containing ubuntu in the name and create your user and password.
In my case: 
```powershell
ubuntu2004.exe
```

Now, let's setup zsh oh-my-zsh, alternative terminals for windows..

First, install Chocolatey has it will be usefull in the future:  (https://chocolatey.org/install)
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Using elevated powershell and choco, let's install Fluent Terminal: (include --force if you run into problems)
```powershell
choco install fluent-terminal
```

Now open Fluent Terminal through start menu and on its settings set up %system32%/wsl.exe as the default.

Now lets symlink a local windows folder as home and a dev folder as well.
```powershell
ln -s /mnt/d/home ~
ln -s /mnt/d/dev ~
```

Inside of home, create a downloads folder.

Lets install exa (alternative for ls). First install rust.
```bash
curl https://sh.rustup.rs -sSf | sh
```

Then lets install cargo.
```bash
sudo apt-get update -y
sudo apt-get install -y cargo
```

Using cargo, let's install exa.
```bash
cargo install exa
# export PATH="~/.cargo/bin:$PATH" 
# (details about this should appear in the notes of the previous command)
# (make sure to include this last command to your zshrc, to allow exa to execute)
```

Install bat (alternative for cat) using cargo:
```bash
cargo install --locked bat
# (if you run "bat" into a file and doesn't work, run the line below, otherwise ignore it)
sudo ln -s ~/.cargo/bin/bat /usr/sbin
```

Install FD as replacement of find: (https://github.com/sharkdp/fd)
```bash
wget https://github.com/sharkdp/fd/releases/download/v8.1.1/fd-musl_8.1.1_amd64.deb
sudo dpkg -i fd-musl_8.1.1_amd64.deb  (remove any conflicting package in case it exists)
```

Install Powerline fonts both linux and windows side.
- On linux, run:
```bash
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```

- On Windows, run:  (make sure Get-ExecutionPolicy is set to Bypass)
```powershell
./install.ps1
```

Now let's install ZSH and GIT:
```bash
sudo apt install git zsh -y
```

And Oh-My-Zsh:
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Let's install powerlevel10k:
```bash
# before that, make sure to set windows terminal to UbuntoMono NF, cause it includes Awesome icons as font
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

[in this meanwhile, let's setup the rest as first time]


....Download dot files repo and run script to replace files.
```bash
ln -s ~/home/dotfiles/.zshrc ~/
ln -s ~/home/dotfiles/.zsh_history ~/
ln -s ~/home/dotfiles/.viminfo ~/
ln -s ~/home/dotfiles/.sudo_as_admin_successful ~/
ln -s ~/home/dotfiles/.zcompdump ~/
ln -s ~/home/dotfiles/.shell.pre-oh-my-zsh ~/
ln -s ~/home/dotfiles/.profile ~/
ln -s ~/home/dotfiles/.oh-my-zsh ~/
ln -s ~/home/dotfiles/.bashrc ~/
ln -s ~/home/dotfiles/.bash_logout ~/
ln -s ~/home/dotfiles/.bash_history ~/
ln -s ~/home/dotfiles/.p10k.zsh ~/
```

Change .zshrc to include this:
```bash
ZSH_THEME="powerlevel10k/powerlevel10k"

plugins=(git zsh_reload sudo pip heroku fd cp)
#you may add other plugins if you wish to do soo

ZSH_DISABLE_COMPFIX="true"

alias exa='~/.cargo/bin/exa'
alias l='exa'
alias la='exa -a'
alias ll='exa -lah'
alias ls='exa --color=auto'
alias cat='bat --style=plain'
```

Installing plugins to oh-my-zsh
```bash
#ZSH Navigation Tools
sh -c "$(curl -fsSL https://raw.githubusercontent.com/psprint/zsh-navigation-tools/master/doc/install.sh)"
```

First time that you run your zsh after changing the Theme, it will do some prompts which you may config as you wish.

This pretty much completes the setup of customization of wsl with zsh, oh-my-zsh, powerlevel10k.
