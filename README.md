# Vagransibled Development VM

Just a bit of R&D POC'ing using vagrant and ansible to spin up a linux+ruby development VM so I can do some ruby development from a Windows machine without that Windows+Ruby headache. 

Deploy on *nix(macos even)? Why would you develop on Windows?
"_....something something_ right tool for the job...."

## Assumptions

- You have [virtualbox](https://www.virtualbox.org/) installed, and working, and doing the things. 
- You have [vagrant](https://www.vagrantup.com/) installed, and working. 

## What It Do
Indeed, what it do? _What. It. Do._ 

It builds up a Rocky 8 VM that can be used as a remote-ssh VSCode host for linux-centric development...that fits my needs, of course. Please, take it and change it if it's useful to you.

## How It Do

Disclaimer: there are probably better ways to go about this (please, tell me about them!)

[Vagrantfile](Vagrantfile) will spin up two Rocky 8 boxes - `dev1` and `ans1`.  `ans1` is a relatively thin VM with the sole purpose is the ansible control node. It can be halted once it's done it's job of provisioning the `dev1` VM. 

`vagrant up` will bring both VMs, and have `ans1` setup the development environment in `dev1`. (**oops**....at the point of writing this, I don't have that all wrapped up.)

## OMG Private and Public Keys

Yes, there is a passphrase-less key pair in [keys/](keys). Big whoop, wanna fight about it?  They're there so the two VMs can freely talk to each other (see [Vagrantfile](Vagrantfile) for details).  Replace them with your own keys. I don't even use the ones here in the repo anymore. They're here so this (hopefully) works right out of the box. 

## WIP

Work in progress... I just want to get this off my machine in case it dies. 
