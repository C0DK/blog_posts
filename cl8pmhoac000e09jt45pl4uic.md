# Configuring Zsh to my liking

## Motivation
I've recently started a new job, with [Pop!_OS](https://pop.system76.com/) rather than Windows (yay!). After a year without using Linux day to day, I wanted to reinstall a bunch of things, one of them being [zsh](https://en.wikipedia.org/wiki/Z_shell), my shell of choice, with [Oh My Zsh](https://ohmyz.sh/) and [Powerline10k](https://github.com/romkatv/powerlevel10k). 

Why?

It adds a bunch of features to the shell that I like:

- Seeing my current branch
- Easier ways to search through history
- Better auto completion.
- sexiness

It might, however, impact performance compared to a clean installation of i.e bash. I don't care about speed. 

## Solution


### Installing: ZSH
Instaling:

```sh
sudo apt install zsh
```

Setting it as a default terminal: 

```sh
chsh -s /usr/bin/zsh
``` 

Now reboot so the shell is changed everywhere. 

You will now be prompted for some fault settings. They aren't super important, but you can tweak it to your needs. I went through it.

### Installing: Oh My Zsh

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Woo! it's now installed. I have a bunch of configurations i'd want to include. You can see my config at the end of this post.

### Installing: Powerline10K

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Install the relevant fonts following the guide [here](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k). 

Change the font in what ever terminal you are using (if gnome terminal, click the burger menu in the top right, go to profiles.. change font)

Then change the theme by changing the `ZSH_THEME` line to `ZSH_THEME="powerlevel10k/powerlevel10k"` in your zsh config file (`~/.zshrc`)

then run `source ~/.zshrc`(or restart your terminal)

Go through the configuration wizard (you can always open it with `p10k configure`) and there you go.

### Plugins i've installed:
- [Zsh autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
-  [Autoswitch Python Virtualenv](https://github.com/MichaelAquilina/zsh-autoswitch-virtualenv)

## Summary
The installation process was fairly easy by simply following the guides on the projects, but sometimes they can be more difficult. I've mainly written this as a reminder for the future. 