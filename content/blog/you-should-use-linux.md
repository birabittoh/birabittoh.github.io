+++
title = "You should probably use Linux"
date = 2021-04-29
template = "blog-page.html"
[taxonomies]
tags = [ "advice", "foss", "privacy" ]
+++

## Why are you writing this?
As you probably know, I like using free software (free as in _freedom_, not free of charge). Most people see users with this mentality as a group of paranoid psychopaths who circlejerk about their custom systems. While that's in part true, there's much more to it.

I often get asked by Windows or MacOS users about why they should think about switching to a GNU/Linux OS (which I'll be referring to as Linux); I'm just going to address everything here so I don't have to repeat it to everyone who asks.

I'll try to address every aspect where Linux is objectively better than the competition, then look at some reasons you _could_ have to stick to Windows or, like I did, set up a dual boot.

## What is free software?
First of all, let's read the official definition for it.

> A program is free software if the program's users have the four essential freedoms:
> 0. The freedom to run the program as you wish, for any purpose.
> 1. The freedom to study how the program works, and change it so it does your computing as you wish.
> 2. The freedom to redistribute copies so you can help others.
> 3. The freedom to distribute copies of your modified versions to others. By doing this you can give the whole community a chance to benefit from your changes.
> A program is free software if it gives users adequately all of these freedoms.

Source: [Free Software Foundation](https://www.gnu.org/philosophy/free-sw.en.html).

Let's examine how this freedom is beneficial.

### Linux is open
Using free software on an open source OS means you always know what's going on with your PC; if you get curious or have any suspects you can always read the source code (or trust that somebody already did it in your place). 

The good thing about Linux is that it doesn't hide anything from you. Whenever there's a problem, you can read various logfiles (with different levels of detail) to identify and troubleshoot your problem; it's also easier to fix problems since you actually know what each program and file does, while troubleshooting in closed-source OSes is like trying to fix a car engine without being able to open the hood.

Some distros, like [Arch Linux](https://archlinux.org/), require you to set everything up from scratch; this means you always know exactly which programs you're installing and their exact function inside the Linux environment. I would only advise this kind of installation to advanced users, but after you do it the first time you'll certainly learn a lot about how a Linux OS actually works.

### Linux is secure
Every single FOSS-oriented website makes this point, I'm just going to re-iterate it just to be sure.
The source code being publicly available doesn't make software less secure. In fact, it's way more secure since more people can work on it and fix security flaws.
[Security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity) just doesn't work. You can see that by looking at the number of security breaches that are found every day on closed-source software.

If you make your software closed-source you're basically betting you and your small team are able to create a better and more secure code than every single other person in the planet. Of course, this assumption is stupid and irrealistic, that's why FOSS software will always be faster and more secure than closed-source alternatives.

This goes for every kind of software, including the very operating system code. GNU/Linux based operating systems are the most secure choice for every kind of user.
More often than not, you can trust the software programmers without even reading the code yourself: since they're sharing every single line of code, they probably don't have anything to hide. If they do include malicious code, someone else will probably have noticed by now, provided you didn't build and run that software straight from the repo a few minutes after the last commit lol

You can also be safe against external attackers. Linux's [market share](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-202012-202012-bar) on desktop and laptop PCs was less than 2% as of January 2021, and those people are probably much more tech-savy than the other OSes' users...
This means attackers will likely target Windows or OS X users, so you can be safe even without using an antivirus or anything similar (even though there _are_ [choices](https://www.clamav.net/) for that, too).

### Linux is smarter
Saying that Linux is for *everybody* would be a risky take. My point is that you _probably_ could benefit from using a Linux system.

If you're a programmer, Linux is objectively the best OS you can use. As a programmer, I love using the terminal to do stuff more quickly. I also love the level of integration you can have with the system: a lot of programs are designed with a client/server model, which makes them work in complex scenarios as long as you have the time and patience to configure them properly.

While Windows still has to retain compatibility with legacy systems, Linux is much more free to do its own thing. Linux will always be smarter and more modern. Just think about the filesystem structure.
Windows is forced to retain a confusing structure, where you have a ton of (not) hidden folders where programmers can store their necessary data... But there are so many choices and they're not coherent! If I wanted to create a backup of all my settings and save files, I would have to copy all of these folders:
```
C:\ProgramData; C:\Users\username\AppData; C:\Users\username\Documents\my games;
```
And I would still miss all of the informations saved on the awful Windows Registry...

On Linux, you just copy the .config folder in your home directory.

Moreover, in Windows 10 you have two ways of editing system settings: the Control Panel and the Windows Settings. But  sometimes editing a setting on one side does NOT reflect on the other!
Windows is literally the most confusing OS you can start with... And people still recommend it to beginners over Linux. It's just dumb.

### Linux is versatile
Now, to the point everybody's been waiting for. Yes, none of the Adobe programs will run _natively_ on any Linux distro. That means if you're a creative person and you need those programs on a daily basis, maybe you should consider dual booting...

BUT, steps are being made in two different directions:
* Valve is working on Proton, which allows the execution of most Windows-only applications and games on any Linux system.
* More and more open-source alternatives to closed-source standards are being developed by the day.

While Proton is interesting, I always prefer to run open source software, especially if we're talking about programs that are also free of charge: imagine trusting a closed-source software you didn't pay for.

## Conclusion
Ok, now I'm getting repetitive so I'll just get to the point.
Linux is constantly evolving and it has now become the top choice for a lot of people, so let's try and consider every use case.

* If you're a professional that's deep in the industry and you need some _specific_ program to run perfectly on your device... Yeah, you should use Windows.
* If you're a power user that's just used to paid software, maybe consider trying out some open source alternative?
* If you're a gamer, I say you should dual boot. I have a Windows 10 LTSC installation that I use _exclusively_ for gaming. While Proton has made Linux gaming feasable, the experience isn't always the best, especially if you play games that require millisecond-grade accuracy, like rhythm games or competitive shooters.
* If you're a student or employee, Linux would be perfect for you. You can quickly take notes and do office work without the annoying Windows 10 updates popping up and rebooting your system seemingly at random. Also, any Linux system will probably be more light on resource usage than Windows, so you could take some old hardware you thought would never be using again and actually make something useful with it.
* If you work in the programming or engineering field, then what are you waiting for? You should try out a Linux OS as soon as possible, and not in a virtual machine. A lot of my friends said they didn't like Linux because it felt slow... While running on a VM... Duh? Try it out on real hardware so you can feel its superiority.

Well, I can't possibly cover *every* profession and use-case, but I hope I was clear about those I managed to list above.
I'm going to conclude this article with some interesting links about Linux and FOSS you definitely should check out.

* [Is Linux About…?](https://islinuxabout.xyz/)
* [usermod.net - Why use Linux?](https://usermod.net/why-use-linux/)
