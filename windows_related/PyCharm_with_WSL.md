# PyCharm on Windows 10 with interpreter from the Linux subsystem (WSL)

- [Step 1: Install WSL](#step1)
- [Step 2: Setup GUI env](#step2)
- [Step 3: Install PyCharm](#step3)
- [Step 4: Create connection](#step4)
- [Step 5: Configuring remote interpreter via WSL](#step5)


## Step 1: Install WSL
Follow the official [instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

## Step 2: Setup GUI env
Install [Xming server](https://sourceforge.net/projects/xming/) to be able to open GUI from bash terminal\
Then, in the bash terminal do: `echo 'export DISPLAY=:0.0" >> ~/.bashrc`

## Step 3: Install PyCharm
Follow the official instructions to install [PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows)

## Step 4: Create connection
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
  Then repeat [step 4](#step4)
  
## Step 5: Configuring remote interpreter via WSL
* Open Pycharm
* Open the [Settings dialog](https://www.jetbrains.com/help/pycharm/configuring-project-and-ide-settings.html), 
and click the [Project Interpreter](https://www.jetbrains.com/help/pycharm/project-interpreter.html) page
* In this page, click gear icon next to Project Interpreter field, and choose Add
* In the left-hand pane of the dialog box, `click SSH Interpreter`, then specify server information 
(host, port, and your username (for Linux)).
![Alt text](images/pycharm_SSH_example.png?raw=true)
