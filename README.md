# dev-server

I use this playbook to get my personal development server up and running.

## Getting Started

To get this running on a fresh Ubuntu server:

    pip install ansible psycopg2
    ansible-pull --url https://github.com/vannoppen-marketing/dev-server/ playbook.yml
    sudo reboot

## Post Install Tasks

Once the playbook completes and the server restarts you can login with your
user. You will then need to add any ssh keys to your .ssh folder and login to
your Heroku account with `heroku login`.

From your host machine:

    scp .ssh/id_rsa .ssh/id_rsa.pub isaac@{{ server_ip }}:~/.ssh/

From your dev machine:

    cat .ssh/id_rsa.pub > .ssh/authorized_keys
    chmod 700 .ssh
    chmod 600 .ssh/*
    heroku login

## Windows Hyper-V Host

If you are running this on a Windows Hyper-V host then the Hyper-V defaut switch
unfortunately continuously changes it's IP upon restart which is "feature".

To counter this you can setup an "Internal Network" in Hyper-V. You'll need to
set a static IP address on the new device created in your Windows network
connections to something like "192.168.10.1" and you'll need to setup a custom
netplan config in your Ubuntu VM. As example create the file
"/etc/netplan/99_config.yaml" and add the following to it:

    network:
      version: 2
      renderer: networkd
      ethernets:
        eth0:
          dhcp4: true
          dhcp6: true
        eth1:
          addresses:
            - 192.168.10.2/24

You can then connect to your Ubuntu machine with it's new internal IP address
"192.168.10.2" and you should have internet access via the default switch still.

## Todo

- Request heroku login info and ssh keys interactively https://docs.ansible.com/ansible/latest/user_guide/playbooks_prompts.html
