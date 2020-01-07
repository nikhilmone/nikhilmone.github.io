# How to install Gitlab-CE on RHEL/ CentOS/ OEL

1. Install and configure the necessary dependencies

[Install the `epel-release` package for addition packages.](https://www.tecmint.com/install-epel-repo-on-rhel-8/)

On CentOS 8 (and RedHat/Oracle/Scientific Linux 8), the commands below will also open HTTP, HTTPS and SSH access in the system firewall.

```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```

Next, install Postfix to send notification emails. If you want to use another solution to send emails please skip this step and configure an external SMTP server after GitLab has been installed.

```
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

During Postfix installation a configuration screen may appear. Select 'Internet Site' and press enter. Use your server's external DNS for 'mail name' and press enter. If additional screens appear, continue to press enter to accept the defaults.

2. Add the GitLab package repository and install the package

Add the GitLab package repository.
```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

Next, install the GitLab package. Change 'https://gitlab.example.com' to the URL at which you want to access your GitLab instance. Installation will automatically configure and start GitLab at that URL.

For https:// URLs GitLab will automatically request a certificate with Let's Encrypt, which requires inbound HTTP access and a valid hostname. You can also use your own certificate or just use http://.

```
sudo EXTERNAL_URL="https://gitlab.example.com" yum install -y gitlab-ee
```

3. Browse to the hostname and login
On your first visit, you'll be redirected to a password reset screen. Provide the password for the initial administrator account and you will be redirected back to the login screen. Use the default account's username root to login.
