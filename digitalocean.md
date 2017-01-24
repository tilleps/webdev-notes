# DigitalOcean Notes #

## Resources ##

- https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04



## Guide ##

https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-16-04


Account creation


1. Create Droplet

2. Check email for server login details

3. Login to server: ```ssh root@0.0.0.0``` it will prompt to change password

4. Disable Password Authentication:

Reload ssh
```sudo systemctl reload sshd```


Upload sysadmin tools

scp -i ~/.ssh/digitalocean.pem -r sysadmin root@0.0.0.0:~/sysadmin

5. Log into server without password (using SSH key)
ssh -i ~/.ssh/digitalocean.pem root@0.0.0.0

cd ~/sysadmin/lib/

./ubuntu/projects-init.sh
./ubuntu/system-install.sh
./linux/pcre-install.sh
./ubuntu/nginx-install.sh
./ubuntu/nginx-setup.sh
./ubuntu/nodejs-install.sh


## Install Node ##
wget https://nodejs.org/dist/v6.9.1/node-v6.9.1-linux-x64.tar.xz
tar -xvf node-v6.9.1-linux-x64.tar.xz
mkdir -p /usr/local/opt/node/6.9.1/
cp -R node-v6.9.1-linux-x64/* /usr/local/opt/node/6.9.1
rm -Rf node-v6.9.1-linux-x64

echo "/usr/local/opt/node/6.9.1/bin" | tee /etc/paths.d/node



