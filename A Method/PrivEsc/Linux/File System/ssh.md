```sh
chmod 600 ~/.ssh/mykey 
ssh-keygen -y -f ~/.ssh/mykey > ~/.ssh/mykey.pub
chmod 644 ~/.ssh/mykey.pub
```