# homestead-Lara

This project provides a vagrant machine for development on the [ILSCeV/Lara](https://github.com/ILSCeV/Lara) calender and personnel scheduling tool.

![homestead-Lara in action](screenshot.png)

## tl;dr

1. Install Git ([Windows](https://git-for-windows.github.io))
2. Install [VirtualBox 5.1](https://www.virtualbox.org/wiki/Downloads)
3. Install [Vagrant 1.9+](https://www.vagrantup.com/downloads.html)
4. Open a console (e.g., Git Bash on Windows, Terminal on Linux). On Windows: start console with elevated rights (as Administrator)
    1. `git clone https://github.com/ILSCeV/homestead-Lara.git`
    2. `cd homestead-Lara`
    3. `init-lara.bat` (Window) / `./init-lara.sh` (Linux/Mac)
    4. `vagrant up`
5. Your Lara Source Code: "homestead-Lara/Code"
6. Your Lara Website: [http://localhost:8000](http://localhost:8000)
7. (Use `restart-vm.bat` to restart the machine if needed or after booting the host system)
8. (Use `destroy.bat` to reset)

## How to rebuild javascript/typescript libraries after your changes
1. Ensure the VM is running (`vagrant up`).
2. Open a shell in your Homstead-Lara directory.
3. Run `vagrant ssh` to ssh to the VM.
4. Switch to the Lara directory `cd /home/vagrant/Code/Lara`.
5. Run `npm run dev` to build `*.js` files and the `mix-manifest.json`.

## Rebuild npm dependencies in case of an error
0. If you are on Windows ensure that you do the following steps as an Administrator.
1. Open a shell in your Homestead directory and run `vagrant up && vagrant ssh` to ssh to the VM.
2. Switch to the Lara directory `cd /home/vagrant/Code/Lara`.
3. Delete old dependencies `rm -r node_modules`.
4. Install dependencies `sudo npm install`.
5. Build Javascript/Typescript fiels `npm run dev`.

## The details

1. Git will be used to download this project and you will use it later to push changes.
2. VirtualBox will be used as a virtualisation driver by Vagrant. You will not interact with it directly.
3. Vagrant is a nifty tool to make creating and setting up a virtual machine a piece of cake. All you really need is one script file (`Vagrantfile`) which describes how the virtual machine should look and feel (OS, packages, mounts, network, portforwardings,...), the rest is done by Vagrant. magic.
4. Choose any console you like. On a windows machine, you have to start this console as an administrator (in the start menu, rightclick and choose "Run as Administrator"). This is important as you need elevated rights, so that `npm install` can create symlinks.
 1. This step downloads this project. It is based on [homestead](https://laravel.com/docs/master/homestead), the official Laravel Vagrant-based development environment. The only modifications are in the files `Homestead.yaml` and `after.sh`.
 2. a not so complicated step.
 3. this will create copies of `Homestead.yaml` and `after.sh` for execution in your personal user folder. Additionally you you will be prompted for the repository and branch to clone from.
 4. This will take some minutes and you will probably get some easily solvable errors. Check out other [useful commands for Vagrant](http://www.erikaheidi.com/blog/quick-user-guide-for-vagrant). That's what happens:
     - Downloads the homestead virtual box.
     - The box will be configured according to the `Homestead.yaml` in your personal folder.
     - The box will boot and at first execute a "provisioner", the script `after.sh` in your personal folder.
     - The script will download the latest lara-vest source code and will initialize the database.
     - After the execution was successful, the virtual machine is active in the background. You can connect and interact with it as with any other virtual machine, you don't have to though.
5. Do your modifications here, the folder is autosynced with the virtual machine
6. **Tadaaa**
7. The script will basically just power down the VM with `vagrant halt` (if currently booted) and (re-)boot the VM with `vagrant up`.
8. The script will destroy the VM, delete the temporary files and delete your vagrant Code content. Caution: all unpushed changes to your Lara repository will be lost.


## Windows, Linux, OSX
The setup was tested under Windows 7 x64 but is supposed to be platform independant.

## Note on upgrades
`homestead-Lara` is based on [`homestead`](https://github.com/laravel/homestead) and the [Homestead Vagrant Box](https://atlas.hashicorp.com/laravel/boxes/homestead).

Both projects are upgraded regulary and because `homestead` as well as `homestead-Lara` always utilize the latest version of the Homestead Vagrant Box and additional external libraries, `homestead-Lara` can behave faulty at any point.
A merge of changes done in `homestead` will be needed!

Please open an issue if you believe that an upgrade is needed.
