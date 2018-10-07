# PyCharm on Windows 10 with interpreter from the Linux subsystem (WSL)

1. **[Install WSL](#isnstall-wsl)**
* **[Setup GUI env](#setup-gui-env)**
* **[SInstall PyCharm](#install-pycharm)**
* **[Create connection](#create-connection)**
* **[Configuring remote interpreter via WSL](#sconfiguring-remote-interpreter-via-wsl)**


## Install WSL
Follow the official [instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

## Setup GUI env
Install [Xming server](https://sourceforge.net/projects/xming/) to be able to open GUI from bash terminal\
Then, in the bash terminal do: `echo 'export DISPLAY=:0.0" >> ~/.bashrc`

## Install PyCharm
Follow the official instructions to install [PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows)

## Create connection
* Open windows `bash` (Run `bash.exe`)
* Do `sudo apt-get update && sudo apt-get upgrade`
* Do `sudo nano /etc/ssh/sshd_config` and change the fields `PasswordAuthentication` to `yes`, and `UsePrivilegeSeparation` to `no`. 
Then save and close
* Do `sudo $(sudo which sshd) -d` to get the information about which SSH daemon is running and where it is listening. 
You should see something like "Server listening on 0.0.0.0 port 22"
* Open a new  windows `bash` and do `sudo ssh 127.0.0.1`. 
You should see password prompt. If you see it, then your server works correctly.
* Start your ssh server in the daemon mode by doing 'sudo service ssh start'
  * If you got something like “Failed to ssh localhost” do the following:\
  add the following into the file `/etc/hosts.allow`:\
  `ssh:ALL:allow`\
  `sshd:ALL:allow`\
  Then,  remove the openssh-server and it again:\
  `sudo apt-get remove openssh-client openssh-server`\
  and\
  `sudo apt-get install openssh-client openssh-server`\
  Then repeat [step 4](#create-connection)
  
  
## Configuring remote interpreter via WSL
* Open Pycharm
* Open the [Settings dialog](https://www.jetbrains.com/help/pycharm/configuring-project-and-ide-settings.html), 
and click the [Project Interpreter](https://www.jetbrains.com/help/pycharm/project-interpreter.html) page
* In this page, click gear icon next to Project Interpreter field, and choose Add
* In the left-hand pane of the dialog box, `click SSH Interpreter`, then specify server information 
(host, port, and your username (for Linux)).
![Like This](/images/pycharm_SSH_example.png?raw=true)
* Click `Next` and select Password or Key pair (OpenSSL or PuTTY). Enter your password or passphrase. Then, click `Next` to proceed with the final configuration step.
* Specify a path to the remote interpeter. 
You have to configure the path mappings to let PyCharm know where to look for files inside of WSL. 
To do that, click the button next to the Sync folders field and enter the path to the local project folder and the path to the folder on the remote server.
  * For simplicity just open the bash.exe, do `whereis python3` or `whereis python2` (depending on the version that you want), 
  copy the path and paste it to the interpreter path.

