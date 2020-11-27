# Install NVM for Node.js

## Step 1:  Update Your System

```bash
sudo apt-get upgrade
```

## Step 2: Download and Install NVM

Now you need to download and install NVM using below command. I have installed the latest Node Version Manager(NVM) from GITHUB Page. At the time of writing this tutorial, v0.35.2 is the latest one. You can go to GITHUB Page and find the latest one to install.

[Download NVM From GITHUB Page](https://github.com/nvm-sh/nvm)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

## Step 3: Restart Terminal

You can restart terminal by below 3 ways. You can use any of them as per your convenience.

__a) Start another bash prompt__

You can start a new bash prompt from current prompt by running exec bash command.

```bash
exec bash
```

__b) Login bash prompt using bash –login__

You can also login to bash prompt using bash –login command.

```bash
bash --login
```

__c) Close and Open Terminal Again__

## Step 4: Check NVM Version

Once you successfully installed NVM in your system, you can check the installed nvm version by using nvm --version command.

```bash
nvm --version
```

## Step 5: Install latest Node.js through NVM

To install node.js in your system through nvm, you need to use nvm install node command. This will install the currently available latest version from repo.

```bash
nvm install node
```

## Step 5 (LTS Version) through NVM

You can also install the latest Node version by using nvm install --lts command as shown below.

```bash
nvm install --lts
```

## Step 6:  Check Node Version

If you want to check the installed node version, you can check by running node --version command.

```bash
node --version
```

## Step 7: List all Installed NodeJs Versions

To check all the currently installed versions of node, you can run nvm ls command. There is an arrow(->) showing in front of the release number which means currently this version is in use.

```bash
nvm ls
```

## Step 8: Switch to a different version.

For example, if you want to switch to Version 13.6.0, then you need to run nvm use v13.6.0 command to switch your node version.

``bash
nvm use v13.6.0
```

## Step 9: Uninstall Version of NodeJs using NVM

If you want to uninstall any particular version let’s say version 8.9, then you need to run nvm uninstall 8.9 command.

```bash
nvm uninstall 8.9
```

## Step 9: Update NodeJs using NVM

If you want to update your node to the latest stable version, then you need to use nvm install stable command. In our case, since the latest version 13.6.0 is already installed in our system so it will show the latest version is already installed and hence no update is currently available to install.

```bash
nvm install stable
```
## Step 10:  Assign Default NodeJs Verion

Example of setting default NodeJs version to 6.1.0

```bash
nvm alias default 6.1.0
```