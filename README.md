# ubuntu_remote_development

Ubuntu server setup for remote development and FTP to local HDDs

Requirement is a computer with ubuntu installed that is connected to the same LAN as your client computer. Written for Ubuntu 22.04 but might work for other versions. 
## Enabling ssh

    sudo apt update
    sudo apt install openssh-server
    sudo systemctl status ssh
    sudo ufw allow ssh
    ip a

## SSH-key

On client end:

    ssh-keygen
    
will generate public key in `~/.ssh/id_rsa.pub`
    
    ssh-copy-id username@remote_host
    
Will place public key in `~/.ssh/authorized_keys` on server end

Make following change `nano /etc/ssh/sshd_config`

    PubkeyAuthentication yes
    AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2
    
then 
    
    sudo systemctl restart ssh
 
Confirm that Pubkey SSH is working then remove password authentication by adding the line

    PasswordAuthentication no

then

    sudo systemctl restart ssh

## Disable sleep on laptop
    
    sudo -H gedit /etc/systemd/logind.conf
    
set `HandleLidSwitch=ignore`

    sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
    
    sudo systemctl restart systemd-logind

`systemd-logind` will mess up graphical session, do `sudo reboot`

    
## Installing nomachine

    sudo apt update
    sudo apt -y install wget
    wget https://download.nomachine.com/download/7.10/Linux/nomachine_7.10.1_1_amd64.deb
    sudo apt install -f ./nomachine_7.10.1_1_amd64.deb
 
 Download for client
 
    https://downloads.nomachine.com/
    
## Setting up FTP on server
Not necessary for remote development.

    sudo apt update
    sudo apt install vsftpd ftp ufw -y
    sudo systemctl enable vsftpd
    sudo systemctl start vsftpd
    sudo systemctl status vsftpd
    ftp localhost

### Cyberduck on client end

Download at https://cyberduck.io/download/

Connect with `SFTP username@remote_host`

## Setup Github SSH on server
    
    git config --global user.email "your_email@example.com" 
    ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    eval $(ssh-agent -s)
    cat ~/.ssh/id_rsa.pub
    
    
Add ssh-key to github

Export clients gitconfig to server

    scp ~/.gitconfig username@remote_host:~/
    
Export alias to server
    
    scp ~/.zshrc username@remote_host~/.bash_aliases
## VS Code remote

On server

    sudo snap install code --classic

On client install https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack

Press `⌘⇧P` and select *Remote-SSH: Connect to Host...* and enter `username@remote_host`
