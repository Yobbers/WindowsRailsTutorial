# WindowsRailsTutorial
How to set up a Rails application on Windows using WSL 2 and Docker.

## Install Guide
This guide is written on Windows 10 Pro Version 2004 Build 19041.508.

### Step 1: Install and enable WSL 2
Install and enable WSL 2 by using the following [guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

### Step 2: Install Ubuntu, Docker and Visual Studio Code
Install [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6), [Docker](https://docs.docker.com/docker-for-windows/install/) and [Visual Studio Code](https://code.visualstudio.com/download) in the previous stated order.

#### Bonus: Visual Studio Code Extensions
In most cases Visual Studio Code automatically detects WSL and Docker and suggests the install of usefull extensions. In case of the opposite, the recommended extensions are: [Docker for Visual Studio Code by Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) and [Visual Studio Code Remote - WSL by Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Close your Visual Studio Code before *Step 3* to make sure that the extensions are active.

### Step 3: Make a Work Directory
Within Explorer visit `\\wsl$\Ubuntu\home\user` and create a folder for your Work Directory.

### Step 4: Open your Work Directory using the Ubuntu terminal and launch Visual Studio Code
First run command `$ cd workdirectory` and then inside your directory run `$ code .`. When run for the first time within your WSL 2, Ubuntu will install server components for Visual Studio Code and launch the application afterwards.

### Step 5: Make a `.dockerignore` and `.gitignore` file inside your Work Directory
Inside Visual Studio Code create two files called *.dockerignore* and *.gitignore* within the Work Directory. Use the pre-made snippets of code inside the designated file: [.dockerignore](https://github.com/wouteryobbers/WindowsRailsTutorial/blob/master/workdirectory/.dockerignore), [.gitignore](https://github.com/wouteryobbers/WindowsRailsTutorial/blob/master/workdirectory/.gitignore).

### Step 6: Make a file named `Dockerfile` inside your Work Directory
Inside Visual Studio Code create a file called *Dockerfile* and use the following snippet:

```dockerfile
FROM ruby

WORKDIR /home/user/workdirectory

ENV PORT 3000

EXPOSE $PORT

RUN gem install rails bundler
RUN gem install rails

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

#Install packages
RUN apt-get update -qq && apt-get install -y build-essential nodejs postgresql-client yarn \
 && rm -rf /var/lib/apt/lists/* \
 && curl -o- -L https://yarnpkg.com/install.sh | bash

ENTRYPOINT [ "/bin/bash" ]
```

### Step 7: Make a file named `docker-compose.yml` inside your Work Directory
Inside Visual Studio Code create a file called *docker-compose.yml* and use the following snippet:

```yaml
version: "3.7"
services:
  ruby_dev:
    build: .
    container_name: ruby_container
    ports:
      - "3000:3000"
    volumes:
      - ./:/home/user/workdirectory
```

### Step 8: Build and Run Container
Open the terminal inside Visual Studio Code and run `$ docker-compose build`. After succesfully building the container run `docker-compose run --service-ports ruby_dev`.

### Step 9: Create a new Rails application
While inside your Container (your terminal should look similar to `root@ab4bb8f693a8:/home/user/workdirectory#`) run `# rails new myapp && cd myapp`.

### Step 10: Update and Install gems
While inside your container and with the application directory selected (your terminal should look similar to `root@ab4bb8f693a8:/home/user/workdirectory/myapp#`) run `# bundle update && bundle install`.

### Step 11: Start Rails Server
While inside your container and with the application directory selected (your terminal should look similar to `root@ab4bb8f693a8:/home/user/workdirectory/myapp#`) run `# rails server -p $PORT -b 0.0.0.0`.

### Step 12: Restarting
After finishing your work day and exiting your container (and computer) by using `$ exit` or `CTRL + D` you will need to restart it the next day. To start this process we firstly need to open our Work Directory within Visual Studio Code by running `$ cd workdirectory && code .`. Then, to make things easier, rename our container by running `$ sudo docker rename old_name_id new_name_id` in the Visual Studio Code terminal. 
Now we are ready to start and enter our newly named container with the command `$ docker start new_name_id && docker exec -it new_name_id bash`. This brings us within our container (notice $ changing to #) and gives us the opportunity to restart our Rails server. First enter your application directory by running `# cd myapp` and then finalize by starting the server with `# rails server -p $PORT -b 0.0.0.0`.
