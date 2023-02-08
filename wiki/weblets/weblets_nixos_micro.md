# Nixos Micro VM

[NixOs](https://nixos.org) Nix is a tool that takes a unique approach to package management and system configuration. Learn how to make reproducible, declarative and reliable systems.

!!!include:weblets_play_go

- Make sure you have an activated [profile](weblets_profile_manager)
- Click on the **Micro Virtual Machine** tab

**Process** :

- Enter the required fields as described in the following images
  ![](img/nixos-micro1.png)

- In Environment variables tab you can and the default configurations for nix for example

```
{ pkgs ? import <nixpkgs> { } }:
let pythonEnv = pkgs.python3.withPackages(ps: [ ]); in pkgs.mkShell { packages = [ pythonEnv ]; }
```

this will be written to `/root/default.nix` where is the place that you can change the nix shell configuration there
![](img/nixos-micro2.png)

- In Disks tab, you should mount a large enough disk for nix to store it's files, used for `nix-store`
  ![](img/nixos-micro3.png)
