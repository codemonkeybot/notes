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

### Step 5 (LTS Version) through NVM

You can also install the latest Node version by using nvm install --lts command as shown below.

```bash
nvm install --lts
```

## Step 6:  Check Node Version

If you want to check the installed node version, you can check by running node --version command.

```bash
node --version
```



