# Add user to Sudoers

## Step 1: Create New User

First we need to create an user which can be given sudo access to perform administrative tasks. We will create a user test for our example. This needs to be done either through root user or any other user having sudo access. Here I am using root user to create test user. Use -m: Create the user’s home directory if it does not exist. 

```bash
useradd -m test
```

## Step 2: Set User Password

You can set test user password using passwd command. Provide a strong password when asked and press enter. Retype the same password and press enter again.

```bash
passwd test
```

## Step 3: Verify User

You can verify user from /etc/passwd file. Here you can grep test user from /etc/passwd file and check if the User is Created or not along with his home directory. You can also verify other details as shown below.

```bash
cat /etc/passwd | grep -i test test:x:1000:1000:test,,,:/home/test:/bin/bash
```
test: Name of the User
1000: User Id
1000: Group Id
test,,,,: Comments Section
/home/test: Test User Home directory
/bin/bash: Shell assigned to the User

## Step 4: Modify User Group

Now you need to add user to the sudo group using usermod command. -a: Add the user to the supplementary group(s). Use only with the -G option. 

```bash
usermod -aG sudo test
```

Now switch to test user and verify if the user is added into the sudo group by running whoami command.

```bash
su - test
sudo whoami
```

## Step 5:  Add User to sudoers using visudo

In most of the cases adding user to the sudo group does the job but it is better to add the user in /etc/sudoers file in case it did not work like below:-

```bash
visudo /etc/sudoers
```

## Step 6:  User is not in the Sudoers File

Sometimes you might see an error user is not in the sudoers file. To resolve this error, you need to check in the /etc/sudoers file if the User is added or not.

```bash
grep -i test /etc/sudoers
```