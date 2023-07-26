<h2 align="center">:snowflake: Ryan4Yin's Nix Config :snowflake:</h2>

<p align="center">
  <img src="https://raw.githubusercontent.com/catppuccin/catppuccin/main/assets/palette/macchiato.png" width="400" />
</p>

<p align="center">
	<a href="https://github.com/ryan4yin/nix-config/stargazers">
		<img alt="Stargazers" src="https://img.shields.io/github/stars/ryan4yin/nix-config?style=for-the-badge&logo=starship&color=C9CBFF&logoColor=D9E0EE&labelColor=302D41"></a>
    <a href="https://nixos.org/">
        <img src="https://img.shields.io/badge/NixOS-23.05-informational.svg?style=for-the-badge&logo=nixos&color=F2CDCD&logoColor=D9E0EE&labelColor=302D41"></a>
    <a href="https://github.com/ryan4yin/nixos-and-flakes-book">
        <img src="https://img.shields.io/static/v1?label=Nix Flakes&message=learning&style=for-the-badge&logo=nixos&color=DDB6F2&logoColor=D9E0EE&labelColor=302D41"></a>
  </a>
</p>

This repository is home to the nix code that builds my systems.

## Why Nix?

Nix allows for easy-to-manage, collaborative, reproducible deployments. This means that once something is setup and configured once, it works forever. If someone else shares their configuration, anyone can make use of it.

**Want to know Nix in detail? Looking for a beginner-friendly tutorial or best practices? Check out [NixOS & Nix Flakes Book - 🛠️ ❤️ An unofficial & opinionated :book: for beginners](https://github.com/ryan4yin/nixos-and-flakes-book)!**

> If you're using macOS, you can also check out [ryan4yin/nix-darwin-kickstarter](https://github.com/ryan4yin/nix-darwin-kickstarter) for a quick start.
## Hyprland + AstroNvim

![](./_img/hyprland_2023-07-26.webp)

![](./_img/hyprland_2023-07-27.webp)

## I3 + AstroNvim

![](/_img/astronvim_2023-07-13_00-39.webp)

## Hosts

```shell
› tree hosts
hosts
├── harmonica  # my MacBook Pro 2020 13-inch, for work.
└── idols
    ├── ai         # my main computer, with NixOS + I5-13600KF + RTX 4090 GPU, for gaming & daily use.
    ├── aquamarine # my NixOS virtual machine with R9-5900HX(8C16T), for distributed building & testing.
    ├── kana       # yet another NixOS vm on another physical machine with R5-5625U(6C12T).
    └── ruby       # another NixOS vm on another physical machine with R7-5825U(8C16T).
```

## How to Deploy this Flake?

> Note: you should NOT deploy this flake directly on your machine, it contains my hardware information and personal information which is not suitable for you. You may use this repo as a reference to build your own configuration.

After installing NixOS with `nix-command` & `flake` enabled, follow the steps below to deploy this flake.

For NixOS, use the following commands:

```bash
# deploy one of the configuration based on the hostname
sudo nixos-rebuild switch --flake .

# we can also deploy using `make`, which is defined in Makefile
make i3

# or we can deploy with details
make i3-debug
```

For MacOS, use the following commands:

```bash
# deploy the darwin configuration(harmonicia)
make ha

# deploy with details
make ha-debug
```

## Install Apps from Flatpak

We can install apps from flathub, which has a lot of apps that are not supported well in nixpkgs.

```bash
# Add the Flathub repository
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# install apps from flathub
flatpak install netease-cloud-music-gtk

# install 3d printer slicer - cura
flatpak install flathub com.ultimaker.cura

# or you can search apps from flathub
flatpak search <keyword>
# search on website is also supported: https://flathub.org/
```

## How to create & managage VM from this flake?

use `aquamarine` as an example, we can create a virtual machine with the following command:

```shell
# 1. generate a proxmox vma image file
nom build .#aquamarine  # `nom`(nix-output-monitor) can be replaced by the standard command `nix`

# 2. upload the genereated image to proxmox server's backup directory `/var/lib/vz/dump`
#    please replace the vma file name with the one you generated in step 1.
scp result/vzdump-qemu-aquamarine-nixos-23.11.20230603.dd49825.vma.zst root@192.168.5.174:/var/lib/vz/dump

# 3. the image we uploaded will be listed in proxmox web ui's this page: [storage 'local'] -> [backups], we can restore a vm from it via the web ui now.
```

Once the virtual machine `aquamarine` is created, we can deploy updates to it with the following commands:

```shell
# 1. add the ssh key to ssh-agent
ssh-add ~/.ssh/ai-idols

# 2. deploy the configuration to the remote host, using the ssh key we added in step 1
#    and the username defaults to `$USER`, it's `ryan` in my case.
nixos-rebuild --flake .#aquamarine --target-host aquamarine --build-host aquamarine switch --use-remote-sudo --verbose

# or we can replace the command above with the following command, which is defined in Makefile
make aqua
```

The commands above will build & deploy the configuration to `aquamarine`, the build process will be executed on `aquamarine` too, and the `--use-remote-sudo` option indicates that we will use `sudo` on the remote host.


## References

Other dotfiles that inspired me:

- Nix Flakes
   - [NixOS-CN/NixOS-CN-telegram](https://github.com/NixOS-CN/NixOS-CN-telegram)
   - [notusknot/dotfiles-nix](https://github.com/notusknot/dotfiles-nix)
   - [xddxdd/nixos-config](https://github.com/xddxdd/nixos-config)
   - [bobbbay/dotfiles](https://github.com/bobbbay/dotfiles)
   - [gytis-ivaskevicius/nixfiles](https://github.com/gytis-ivaskevicius/nixfiles)
   - [fufexan/dotfiles](https://github.com/fufexan/dotfiles)
   - [davidtwco/veritas](https://github.com/davidtwco/veritas)
   - [gvolpe/nix-config](https://github.com/gvolpe/nix-config)
   - [Ruixi-rebirth/flakes](https://github.com/Ruixi-rebirth/flakes)
- Hyprland
   - [HeinzDev/Hyprland-dotfiles](https://github.com/HeinzDev/Hyprland-dotfiles)
   - [notwidow/hyprland](https://github.com/notwidow/hyprland)
- I3 Window Manager
   - [denisse-dev/dotfiles](https://github.com/denisse-dev/dotfiles)
- Neovim/AstroNvim
   - [maxbrunet/dotfiles](https://github.com/maxbrunet/dotfiles): astronvim with nix flakes.
- Theme
   - [catppuccin](https://github.com/catppuccin/catppuccin)
- Misc
   - [1amSimp1e/dots](https://github.com/1amSimp1e/dots)
