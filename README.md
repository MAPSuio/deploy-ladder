# Deploy-ladder

This repository contains scripts and instructions for deploying
[our own fork](https://github.com/MAPSuio/programming-ladder.git) of
[Programming Ladder](https://github.com/alexanbj/programming-ladder).

# Prelminaries

## Create an SSH keypair

Example:

```
ssh-keygen -f ~/.ssh/digitalocean -C "keypair for digitalocean"
```

## Create a Droplet on DigitalOcean

Choose *Ubuntu* version *15.10* or later. Use *x64*, and choose
a configuration that has *at least 2GB of RAM*.

Choose the datacenter that is closest to where the competition will
be held. If you're hosting the competition in Oslo, go with *Amsterdam*.

Add the *public* SSH key you generated in the previous step.

## Make your first connection and install Python

Write down the IP address of the droplet.

```
ssh root@<ip-address> -i ~/.ssh/digitalocean
```

You should see something like this:

```
Welcome to Ubuntu 15.10 (GNU/Linux 4.2.0-27-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
Last login: Sat Mar 12 12:22:24 2016 from 85.165.146.108
root@programming-ladder:~#
```

Proceed with installing Python. Python is needed for Ansible:

```
root@programming-ladder:~# apt-get install python
```

## Configure Ansible

Copy `hosts.example` to `hosts`, and replace the IP address in `ansible_host`
with the IP address of your Droplet.

Test your connection by issuing the following command:

```
ansible -m ping digitalocean
```

If you see something like the following, you can move onto the next step:

```
digitalocean | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Run the Ansible Script

```
ansible-playbook deploy.yml
```

When the script finishes, the site is available at `http://<droplet-ip-address>`
after a little while.

### Creating the Programming Ladder administration user

The first time you run Programming Ladder, log in. The user you create
will be the administrator user when you restart programming ladder. To
restart programming ladder, type `service ladder restart`.

## Monitor the process

SSH into the server, and then Run
* `service ladder status` to inspect the log
* `service ladder stop` to stop the service
* `service ladder start` to start the service
