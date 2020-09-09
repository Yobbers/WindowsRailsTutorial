# WindowsRailsTutorial
How to set up a Rails application on Windows using WSL 2 and Docker.

## Install Guide
Using Windows 10 Pro Version 2004 Build 19041.508 at the time of writing latest version.

### Step 1: Install and enable WSL 2
Install and enable WSL 2 by using the following [Microsoft docs](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

### Step 2: Install Ubuntu, Docker and Visual Studio Code
Install [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6), [Docker](https://docs.docker.com/docker-for-windows/install/) and [Visual Studio Code](https://code.visualstudio.com/download) in the previous stated order.

### Step 3: Make a Work Directory
Visit `\\wsl$\Ubuntu\home\user` and create a folder for your Work Directory.

### Step 4: Open your Work Directory using the Ubuntu terminal and launch Visual Studio Code
First run command `$ cd workdirectory` and then inside your directory run `$ code .`. When run for the first time within your WSL 2 Ubuntu will install server components for Visual Studio Code and launch the application afterwards.

### Step 5: Make a `.dockerignore` and `.gitignore` file inside your Work Directory
Inside Visual Studio Code create two files called .dockerignore and .gitignore within the Work Directory. Use the pre-made snippets of code inside the designated file: [.dockerignore](), [.gitignore]().
