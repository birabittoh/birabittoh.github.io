+++
title = "Modernizing XFCE"
date = 2023-02-05
template = "blog-page.html"
[taxonomies]
tags = [ "foss", "advice" ]
+++

This article is more of a memo for myself. Since I often re-install Linux systems, I spend a lot of time doing repetitive tasks and often forget some steps, which leads me to waste even more time figuring out what's wrong.

These instructions allow you to get a fully functional and modern-looking XFCE desktop on a fresh install of [Void Linux](https://voidlinux.org/) (even though they can be adapted to work in any distro).

## Initial system update
First thing I tried was to update the system, but the ISO was quite old. I had to update `xbps` before anything else:

```
sudo xbps-install -Su xbps
sudo xbps-install -Su
```

## Avoid session saving
One thing I hate about XFCE is its fixation to save sessions. A lot of times I get my session saved and restored even with all settings turned off.

A quick and easy solution to disable session saving entirely is just to create an empty file in place of the `sessions` directory.
```
rm ~/.cache/sessions -rf
touch ~/.cache/sessions
```

This way, even with everything turned on, XFCE fails create a folder with that name and everything works (or doesn't, in this case) like a charm.

## Change that shell
Your shell is the main tool you use to communicate with your system, so it makes sense to replace `bash` with something more modern and feature-rich.

```
sudo xbps-install -S zsh zsh-completions curl
chsh -s /bin/zsh
zsh
curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
```

## Get some good sound quality
I don't like `pulseaudio`. Let's replace it with `pipewire` and `wireplumber`.

```
su
xbps-install pipewire wireplumber
mkdir -p /etc/pipewire/pipewire.conf.d
sed '/path.*=.*pipewire-media-session/s/{/#{/' \
/usr/share/pipewire/pipewire.conf > /etc/pipewire/pipewire.conf
echo 'context.exec = [ { path = "/usr/bin/wireplumber" args = "" } ]' \
> /etc/pipewire/pipewire.conf.d/10-wireplumber.conf
ln -s /usr/share/applications/pipewire* /etc/xdg/autostart
xbps-remove pulseaudio alsa-plugins-pulseaudio
reboot
```

## Make Firefox more secure
I like Firefox as a browser, but it doesn't come with sane defaults as far as privacy's concerned.

First, visit [Firefox Profilemaker](https://ffprofile.com/) and create a customized `profile.zip`.

Then, extract your zip file to the correct destination:
```
sudo xbps-install zip unzip p7zip xarchiver thunar-archive-plugin
unzip -o ~/Downloads/profile.zip -d ~/.mozilla/firefox/xxxx.default-default/
```

Some useful extensions I always install are:
- [Bitwarden](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager), a password manager;
- [Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes), to serve common JS libraries locally;
- [I still don't care about cookies](https://addons.mozilla.org/en-US/firefox/addon/istilldontcareaboutcookies), to hide and auto-reject cookie warnings;
- [LibRedirect](https://addons.mozilla.org/en-US/firefox/addon/libredirect), a redirector for [alternative front-ends](https://github.com/mendel5/alternative-front-ends);
- [SponsorBlock](https://addons.mozilla.org/en-US/firefox/addon/sponsorblock), to skip YouTube sponsorships automagically;
- [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin), the best ad-blocker.

## Customize your DE

Install the last required packages for desktop usability:
```
sudo xbps-install -S vpm xfce4-whiskermenu-plugin xfce4-clipman-plugin \
xfce4-pulseaudio-plugin xfce4-screenshooter xclip micro neovim mpv yt-dlp
```

Remove orphaned and cached packages:
```
sudo xbps-remove -Oo
```

Now, open XFCE's Settings Manager and set the following options:
- Appearance &rarr; Style &rarr; Choose "Adwaita-dark"
- Desktop &rarr; Background &rarr; _&lt;Choose your favorite wallpaper&gt;_
- Desktop &rarr; Icons &rarr; Set "Icon type" to "None"
- Panel &rarr; _&lt;Customize your panels&gt;_
- Screensaver &rarr; Disable "Enable Screensaver"
- Text Editor Settings &rarr; Enable:
    - "Show line numbers",
    - "Highlight matching brackets",
    - "Wrap long lines"
- Window Manager &rarr; Style &rarr; Button layout &rarr; Remove "Shade" button from title bar
- Window Manager &rarr; Advanced &rarr; Windows snapping &rarr; Enable "To other windows"
- Window Manager &rarr; Advanced &rarr; Wrap workspaces when reaching the screen edge &rarr; Disable "With a dragged window"
- Window Manager Tweaks &rarr; Cycling &rarr; Enable:
    - "Cycle through minimized windows in most recently used order",
    - "Cycle through windows on all workspaces",
    - "Raise windows while cycling"
- Window Manager Tweaks &rarr; Accessibility &rarr; Disable:
    - "Raise windows when any mouse button is pressed",
    - "Use mouse wheel on title bar to roll up the window"
- Window Manager Tweaks &rarr; Accessibility &rarr; Enable "Notify of urgency by making window's decoration blink"
- Window Manager Tweaks &rarr; Compositor &rarr; Enable "Show shadows under popup windows"
- Xfce Terminal Settings &rarr; General &rarr; Scrolling &rarr; Set "Scrollbar is" to "Disabled"
- Xfce Terminal Settings &rarr; Appearance &rarr;
    - enable "Use system font",
    - set "Background" to "Transparent background",
    - set Opacity to 0.80;
- Xfce Terminal Settings &rarr; Colors &rarr; Presets &rarr; Choose "Tango"
- Keyboard &rarr; Behavior &rarr; Enable "Restore num lock state on startup"


## Shortcuts
Finally, set the following shortcuts:
- Keyboard &rarr; Application Shortcuts

| Command                                                                                          | Shortcut                |
|--------------------------------------------------------------------------------------------------|-------------------------|
| `exo-open --launch TerminalEmulator`                                                             | `Super` + `Return`      |
| `xfce4-popup-whiskermenu`                                                                        | `Super`                 |
| `xfce4-screenshooter --clipboard --region`                                                       | `Shift` + `Super` + `S` |
| `xfce4-screenshooter --clipboard --window`                                                       | `Super` + `S`           |
| `xfce4-screenshooter --clipboard --fullscreen`                                                   | `Print`                 |
| `sh -c 'xclip -selection clipboard -t image/png -o > "$HOME/Pictures/$(date +%Y-%m-%d_%T).png"'` | `Shift` + `Super` + `V` |
| `xflock4`                                                                                        | `Super` + `L`           |
| `loginctl suspend`                                                                               | `Shift` + `Super` + `L` |
