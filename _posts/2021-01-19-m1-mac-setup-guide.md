---
title: M1 Mac Setup Guide
author: Onur Genes
date: 2021-01-19 17:58:00 +0300
tags: [macos, m1, guide]
categories: [M1, Guide]
math: false
toc: true
image: 
    src: /assets/img/m1.jpg
    alt: Apple M1 Chip
---

So you have bought the shiny new M1 Macbook Pro, Macbook Air or Mac Mini. What now? It is a different architecture for CPU wise. It is not a regular Intel CPU. Not every app or program you are using is compatible. Rosetta 2 is working fine. Most of the you can't tell the difference but sometimes things can get trickier.

Let's start from the moment you have bought it.

# Updates

Before touch into anything, there will be updates depending on the date you have bought. For me, it has updates for MacOS and app updates as well.

So, hold your horses and go to **System Preferences > Software Update**. Start update. It might take a while depending your internet connection but still, you have to do it.

Done already? Let's continue.

# General stuff

You should start changing internal apps settings.

## Safari

If you are a developer you might want to open Developer tools. Maybe you were using an ad blocker *(you should)* go and download it from App Store. I recommend [Wipr](https://apps.apple.com/us/app/wipr/id1320666476?mt=12) because it is really lightweight and easy to use. Of course I am also using [Pi-Hole](https://pi-hole.net/).

If you are interested you can check [my other post](/posts/Being-a-Privacy-Activist-as-a-Developer/).

## Email

Personally I don't like using Apple Mail. I am using [Spark](https://sparkmailapp.com) but I will change it soon probably. It might be a good time to try Apple Mail again. But if you use custom mail app, go to **Mail.app > Settings** and you will see the default mail app. Change it.

## System Preferences

Change the Keyboard, Trackpad and Mouse settings. If you used everything in default until today, you should try playing around with settings. For example, **Key Repeat** and **Delay Until Repeat** is must be at the top for me. Otherwise it feels so slow.

Also if you are a bilingual like me, add new **Input Types** as well. **You can switch between languages using ⌘ + ⌥ + Space**.

# If you are a developer

## Xcode

You should start everything with installing Xcode. You can download it from App Store but also from [Apple Developer Website](https://developer.apple.com)

## iTerm

I am using [iTerm](https://iterm2.com). It is a really good Terminal app. It is supporting custom fonts and also M1 supported out of the box. Go for it.

## Command Line Tools

Don't forget to install Command Line Tools. You can install it with `xcode-select --install`. It will take a while but it is better to have it before hand.

## oh-my-zsh

MacOS using `zsh` as default. If you want to customize it easily you can use [`oh-my-zsh`](https://ohmyz.sh). Which is my favourite.

## powerlevel10k

I was a long time user of [powerlevel9k](https://github.com/Powerlevel9k/powerlevel9k) but it is deprecated but luckily we have [powerlevel10k](https://github.com/romkatv/powerlevel10k). Which is more powerful and fast, as the name suggests.

Also it is much more easier to install because it install everything by itself. Just follow the instructions. Also you will have a fancy Terminal. Cool!

## zsh-autosuggestions and zsh-syntax-highlighting

These are the best plugins for the `oh-my-zsh`. Using them will give you super powers! Well, sort of. If you are forgetting commands all the time, yes they will give you super powers.

## zsh-nvm (if you are using NodeJS)

Of course everyone uses NodeJS. Go and install [`zsh-nvm`](https://github.com/lukechilds/zsh-nvm). It will do everything necessary for you. And you will have `nvm`. It is supercool. We will need this.

After installing `nvm`, install NodeJS. But beware, it will compile everything from source. Beacuse at the time I have written this post, there were no pre-built binaries for `arm64` on `nvm`.

It will take time but you have to do it anyway.

## Homebrew

You were waiting for this, don't you? Here we are.

I have followed [Sam Soffes's Blog](https://soffes.blog/homebrew-on-apple-silicon) for the tutorial but I will give you a shorter version.

Just copy the commands below:

```sh
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
sudo mkdir -p /opt/homebrew
sudo chown -R $(whoami):staff /opt/homebrew
cd /opt
curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
```
and then open your `.zshrc` file with `nano ~/.zshrc` (yes, I am using nano).

Last thing is adding following lines at the end of the `.zshrc` file:

```sh
export PATH="/opt/homebrew/bin:/usr/local/bin:$PATH"
alias ibrew='arch -x86_64 /usr/local/bin/brew'
```

You can change `ibrew` to `brew_old_and_slow_intel` or whatever. It is your decision to how to praise your lovely **_M1 Macbook Pro_**.

## VS Code

Every developer needs a slow, feature bloated and (too much) customizable Text Editor, right? We will use [VSCode Insiders](https://code.visualstudio.com/insiders/) because it has **native M1 support**. Lovely!

Go [here](https://code.visualstudio.com/insiders/) and click that sooo small `ARM64` link.

Also funny side effect, `Activity Monitor` shows `VSCode` as `iOS` app. I wish it was true!

## The Others

I will not guide you through all the tools I am using but putting a little list below.

* [Polypane](https://polypane.app) - This is one of the best Web Developer tools.
* [Insomnia](https://insomnia.rest) - Light weight HTTP Rest app. Better than Postman.
* [Alfred](https://www.alfredapp.com) - Are you still using Spotlight? What a shame!
    * Don't forget to change default search engine. [Here is a tutorial](https://www.mahal.org/2016/08/change-default-search-alfred/)
* [BetterTouchTool](https://folivora.ai) - Especially if you are using a Macbook Pro with a **beautiful Touch Bar**.
* [Proxyman](https://proxyman.io) - Charles Proxy alternative. It is a native Mac App and supports M1 out of the box!
* [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
    * Don't forget to add [Firefox Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/). You will thank me later.
* [Termius](https://www.termius.com) - If you are using AWS or using so many different `SSH Keys`, you should consider this.

# Last Touches

If you are using a Mac, you should use Native Mac Apps. Not Adobe stuff.

They are lighter, faster, better, stronger and **_cheaper_**. Go to [Affinity Website](https://affinity.serif.com/en-gb/) and buy everything. Seriously. They are really good.

# That's It

Probably we have forgotten so many things but still, having this kind of list is handy.

Thanks for reading.