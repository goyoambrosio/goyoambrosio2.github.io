---
title: "How to install and get updated Visual Studio Code in Ubuntu 16.04"
related : true
categories:
  - HowTo
tags: 
  - visual studio
  - code
  - microsoft
  - ubuntu
  - install
  - update
---

![Image title](/assets/images/2017/03/VisualStudioCodeLogo.png){: .align-left}If you are getting in trouble while trying to install and/or update Visual Studio Code (*Version 1.10 is available while I'm writing this little post*) in Ubuntu 16.04 copy'n'paste next command in your favourite terminal

    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg && \
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg && \
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list' && \
    sudo apt-get update && \
    sudo apt install code code-insiders
    
You can get more info in [Visual Studio Code PPA](https://github.com/tagplus5/vscode-ppa) GitHub repository or [Running VS Code on Linux](https://code.visualstudio.com/docs/setup/linux#_installation) official Microsoft site.
