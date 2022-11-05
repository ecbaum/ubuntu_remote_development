# ubuntu_server_installation


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
    
## No sleep
    
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
