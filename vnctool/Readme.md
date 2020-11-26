# VNCTool

vncdotool is a command line VNC client. It can be useful to automating interactions with virtual machines or hardware devices that are otherwise difficult to control.

Install vncdotool [vncdotool](https://github.com/sibson/vncdotool)

```bash
sudo pip install --trusted-host=pypi.org --trusted-host=files.pythonhosted.org vncdotool
```

Browser Refresh Example - Replace 127.0.0.1 with appropiate ip address - replace 5901..5999 with appropiate vnc port address ranges

```bash
for i in {5901..5999};do echo 'vmname-here' $i;vncdotool -p "vncpass" -s 127.0.0.1::$i key f5;sleep 10;done &
```