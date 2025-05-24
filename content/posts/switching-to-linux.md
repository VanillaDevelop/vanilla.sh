+++
date = '2025-05-24T20:00:00+01:00'
title = 'Switching to Linux: An Experience'
tags = ['Linux', 'Operating Systems', 'SysAdmin']
+++

## Introduction
This is mostly just a quick rant/blog post. I've finally decided, after decades of Windows use, to switch my main operating system to Ubuntu. Among the reasons for this were easier access to dev tools, more customizability, and being absolutely fed up with the random nonsense that Windows 11 shoves down my throat. Hell, even when reinstalling Windows to my second drive as a backup dual-boot solution, I could feel myself getting more and more annoyed by the increasing amount of late-stage capitalism BS asked of me on every screen, from being forced to log into a Microsoft account, to 30 different "customisation" options (do you want targeted ads???), and the familiar sight of this random news sidebar menu that nobody ever asked for nor used.

So, let's see what my first few weeks of Linux taught me.

## The Good
### Developer Tools Are Better
First things first, there is a reason why developers generally prefer Linux systems. It was super easy to use the command line to set up common developer tools like Git, Python, and SSH keys for my GitHub account. It's stuff that usually requires at least some messing around with path variables on Windows. JetBrains also got me, with full Linux support for their IDEs and the Toolbox. Oh, and with the increased use of CLI, I finally had a reason to set up and use Neovim properly. I will probably update my [developer setup guide](https://vanilladevelop.github.io/IDE-setup/) soon to reflect the Linux settings.

I know that's not really all that amazing of a feat, doing basically the bare minimum to get a development environment working. But I do think that these small benefits, as well as better access to open-source tooling, will eventually add up over time and lead to a much better overall experience.

### Gaming Is Not That Bad Anymore
Since the old days of Wine, Linux has come pretty far, and there are now tools like Lutris, Bottles, Winetricks, Heroic, and Proton that manage a lot of the more painful stuff from back in the days for you. I haven't fully figured out all the intricacies of these tools yet, but using Lutris, I was able to get WoW and Hearthstone to run pretty smoothly, and Steam with its support for custom compatibility layers allowed me to set up FFXIV as well, using the power of the usual Dalamud nerds and whatever they cooked up. For Epic Games, I installed the Heroic Games Launcher, which, at least for Singleplayer games, seems to do the job just fine.

I ran into a bit of a red herring early on with NVIDIA drivers - in general, it seems like NVIDIA support is not as great as AMD for Linux, and I appeared to run into an issue where Battle.net would not install. According to the [Readme File](https://github.com/lutris/docs/blob/master/Battle.Net.md) this is caused by incorrectly installed graphics drivers, driving me to reinstall the whole system (luckily, installing Battle.net was one of the first things I did!). However, in the end, it was actually caused by a completely different recent and unrelated bug described [here](https://www.reddit.com/r/Lutris/comments/1kbx0lq/battlenet_update_agent_strikes_again/), which I fixed by simply using a newer Wine version. Specifically, I installed ProtonPlus using Flatpak and downloaded the 10.7 Wine version for Lutris to use (This can be specified in the _Runner Options_ of the specific entry).


Not sure what the takeaway here is, ironically enough ChatGPT's search mode discovered these posts when regular Google wouldn't. I haven't decided yet if it's scary that AI is now finding things that I can't find myself, or if I never would've found them before AI to begin with, but I guess it is technically a tool that ended up helping - more on the AI singularity in another post.

Anyway, small quirks aside, gaming works mostly fine on Linux, and the few games that don't work out of the box can always be played on a dual-booted Windows system.

## The Bad
### Getting Rid Of Proton
I've been a customer of [Proton Unlimited](https://proton.me/) for a bit over a year now. However, for a company that continuously prides itself on privacy and security, I was amazed to learn that they do not offer Linux clients for some of their tools, such as Proton Drive. Since backing up my local files is a big driver in letting me hot-swap operating systems as I did here, I decided to swap to different services altogether.

**Proton Drive → Google Drive**  
Something I never really noticed just having Proton Drive run in the background is how damn SLOW it is for actually downloading files. At times, it took me hours to painstakingly download a 2GB file at the astonishing rate of several kilobytes per second. Now, Proton Drive is cheap, especially when it comes as part of Proton Unlimited, but I found a different way to keep my overall budget the same: I swapped out my ChatGPT subscription for a Gemini subscription, which comes with Google Drive storage as well, and I just moved the whole thing over to Google Drive. Pretty much the opposite of privacy, I'll admit, but the files I store in the cloud aren't really sensitive to begin with. I think in the medium-term future I'll be looking to store these things on a NAS at home and only occasionally upload encrypted backups to the cloud. But for now, this is fine.

I use _rclone_ with Google Drive as a remote directory to perform a one-way synchronization using cronjobs. The neat part is that this allows me to periodically, based on importance, sync certain folders into different directories. For example, this is how I set up my crontab to periodically backup game settings while also synchronizing my main directory more frequently:

```bash
# Every Saturday and Sunday at 1am, sync the FFXIV main character folder
0 1 * * 6,0 rclone sync "$HOME/.xlcore/ffxivConfig/FFXIV_CHR0040002E92C8FF28" gdrive:_Games/FFXIV
# Every Saturday and Sunday at 2am, sync the WoW Interface and WTF folders
0 2 * * 6,0 rclone sync "$HOME/Games/battlenet/drive_c/Program Files (x86)/World of Warcraft/_retail_/WTF" gdrive:_Games/WoW/WTF
0 2 * * 6,0 rclone sync "$HOME/Games/battlenet/drive_c/Program Files (x86)/World of Warcraft/_retail_/Interface" gdrive:_Games/WoW/Interface
# Every 15 minutes, attempt to sync the local Data directory into Google Drive
*/15 * * * * rclone sync "$HOME/Data/" gdrive:_Data
```

**Proton Pass → BitWarden**  
I've never particularly liked Proton Pass and its browser extensions. It has felt extremely hit or miss with filling information on websites (although this can probably at least partially be attributed to the abysmal web design that is all across websites in the current year...like, just give me a normal login screen with a username and password field...) For now, I've migrated to BitWarden, since it's free and has good support across all platforms. Proton Pass was a relatively inoffensive product, but I don't exactly miss having it, either.

**Proton Mail → iCloud**  
Honestly, good mail servers are a pain in the ass, and Proton actually delivered a pretty good value proposition here. Mail was the first Proton service I used, and I quite liked it. However, while encrypted mail is nice, what's not so nice is not being able to add the emails remotely to any other app without running your own provider. Since switching to Apple products, I've mainly been consolidating my emails in the Mail app, and my Proton-powered domains were noticeably missing from this setup.

I migrated to using iCloud as an email provider for my domains, which was already included with my iCloud plan anyway, allowed me to keep using catch-all e-mails with my domain, and finally let me have everything in one neat app at no extra cost.

### Where's My Social Tooling
I share a lot of images and clips online. Windows has great tooling for this: ShareX for images, and tools like Medal.TV for clipping and sharing videos. Neither of those exists on Linux, so I had to find some alternatives.

For ShareX, I installed [ksnip](https://github.com/ksnip/ksnip), which allows for custom uploading of images via a script. I had to disable the built-in screenshot tool, so I could use the print screen keybind, then set up the following custom script to upload to my own image-hosting solution [nya.gg](https://nya.gg):

```bash
#!/bin/bash

set -e

IMAGE_PATH="$1"
UPLOAD_URL="https://nya.gg/i/"
API_KEY="MY_API_KEY"

RESPONSE=$(curl -s --request POST --url "$UPLOAD_URL" --header 'Content-Type: multipart/form-data' --header "X-API-Key: $API_KEY" --form file=@$IMAGE_PATH)

if [ $? -ne 0 ]; then
    echo "Error: Curl command failed. Exit status: $?. Response: $RESPONSE" >&2
    exit 1
fi

IMAGE_URL=$(echo "$RESPONSE" | jq -r '.url')

if [ -z "$IMAGE_URL" ] || [ "$IMAGE_URL" == "null" ]; then
    echo "Error: Could not extract URL from response. Response: $RESPONSE" >&2
    exit 1
fi

echo "$IMAGE_URL"
```

Finally, I created a custom action to capture the image, save it to a local drive (which is also synced to Google Drive as per the previous chapter), and upload it to my website. I then set up ksnip to run on launch using the "startup applications" tool and that was that! Sounds easy, took me a while to figure out though (especially because I forgot my own API structure and hadn't documented it...)

For recording clips, I simply use OBS Studio now. It has some advantages and drawbacks. What's nice is having better native quality and more fine-grained control over what's recorded. However, the obvious disadvantage is losing the backing framework of instant upload and sharing. Maybe I'll try to find or code a tool like this myself in the future since I would like to have these clips in a better place than Medal anyway (I still have PTSD from what happened to plays.tv). For now, the OBS Studio replay buffer is probably fine.

## The Ugly
Finally, let me tell you about some wacky bugs I had to fix so far.

### Quirky Lutris Settings
Lutris does have some quite opinionated configurations that can become pretty annoying. On Battle.net, I had to add the following environment variable in system options: `XMODIFIERS`=`@im=fcitx`, so that some of my QWERTZ hotkeys would work properly. In general, quite a few applications don't seem to play nice with QWERTZ shift hotkeys (likely due to the symbols they produce, but there is also a surprising lack of consistency - Shift+3 is somehow the only shift keybind that works on ksnip, but won't work under Lutris at all).


### What The Heck Is Wayland
I was quickly introduced to the difference between X11 and Wayland when I ran into some [quite annoying bug](https://www.reddit.com/r/linux_gaming/comments/qaxz3m/comment/hqankhn/?context=3) that causes micro stutters when serving consecutive key inputs from different devices, which gets old really quickly when you're gaming with an MMO mouse. Luckily, some people [already wrote up a solution here ](https://gitlab.gnome.org/GNOME/gnome-shell/-/issues/1858#note_818548) as well as [here](https://blog.aloni.org/posts/how-to-easily-patch-fedora-packages/) that works by changing one singular line in the mutter package. So, while I was at it, I got a lesson in re-compiling packages as well...

Below, I've written down the steps I took to fix the issue for me:

- `apt show mutter` to get my local mutter version - displays `46.2-1ubuntu0.24.04.8`
- In "Software & Updates" => Ubuntu Software: Toggle "Source Code" on
- `sudo apt update`
- `apt source mutter` to get the mutter source files
- `sudo apt build-dep mutter` to install the build dependencies
- Open the `/src/backends/x11/meta-backend-x11.c` file
- Delete the switch case `XkbNewKeyboardNotify`
- `sudo apt install build-essential devscripts debhelper`
- Run `dpkg-buildpackage -us -uc -b` in the main directory
- After the build, run `sudo apt install ./libmutter-14-0_46.2-1ubuntu0.24.04.8_amd64.deb` (see version from above)
- Some dependencies were complaining after this, so I ran `sudo apt --fix-broken install`
- Then `sudo apt-mark hold libmutter-14-0` so the library won't get automatically updated
- Followed by `sudo apt-get update`, `sudo apt-get upgrade`, `sudo apt-get autoremove`

Knowing my ability to break Linux systems, in about 2 months, I'll probably need this again, so I figured it would be a good idea to write it down.

Also, no, I can't switch to Wayland (which seemingly doesn't have this issue), because it apparently causes a whole host of other issues, especially on NVIDIA cards, and I've already begun noticing some in a single session of trying it out, such as weird flickering when scrolling in apps.


## Some Final Words
This is just an excerpt of some of the early nonsense I've had to deal with while setting up Linux. I doubt it would make sense to keep extending this article as I keep customizing the system (although I've played with the idea of making some type of repository of issues I've encountered that took me a while to fix...possible future project idea), but it's definitely not a super painless process.

People won't want to hear this in an "AI bad" type of environment, but honestly, using AI to understand Linux quirks better is extremely effective, especially with synthesizing sources for obscure problems. But end of the day, the only way you learn something is by doing it, and no amount of Linux sysadmin books can prepare me for that. It's entirely possible that some people are cringing just reading this article, considering most of what I did is presumably either very basic stuff or already broke something it wasn't supposed to, but I'll probably make that experience myself in a year or two.

Anyway, this is just a quick update on what I've been up to! Naturally, this whole process ate into a lot of my coding time, but I should be back to working on projects soon now that I've set up most basic stuff and future-proofed my personal tooling.

At least I don't have to see the news sidebar anymore.