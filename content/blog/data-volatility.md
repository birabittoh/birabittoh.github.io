+++
title = "Data volatility"
date = 2022-01-14
template = "blog-page.html"
[taxonomies]
tags = [ "privacy", "foss", "advice" ]
+++

I tried to access my domain at smol.pub yesterday and I noticed the service went down. Fear started rushing through my veins as I noticed I would have to choose another platform and, most importantly, write everything back from scratch since I don't have a backup. This made me think about the importance of always having a backup stored somewhere.

## Why though
Creating a backup of your important data is crucial. On a daily basis, people discover vulnerabilities that allow remote code execution on any host machine. Try to imagine what would happen if someone ran a ransomware program on your PC. Would you be safe?

This genuinely feels like fearmongery, but it's something that can seriously happen: you can be attacked by someone that specifically targets you. If you run Windows, you might be part of a botnet (think about all of the unsigned EXE files you've run since you installed the OS). What happens when someone doesn't need your machine anymore? Well, that person might try and squeeze some money from you by holding your files hostage.

## Cloud backups
Most people define cloud storage as follows:
> Cloud storage is a way for businesses and consumers to save data securely online so that it can be accessed anytime from any location and easily shared with those who are granted permission. Cloud storage also offers a way to back up data to facilitate recovery off-site.

Source: [Investopedia](https://www.investopedia.com/terms/c/cloud-storage.asp).

In reality, cloud storage is no more than some dude's computer.

As soon as you upload your personal data to any service, you're trusting it to store it in a safe and private way. If that software is not open source, you're basically asking to get spied on.

Most people do not care about that, that's why cloud storage solutions are very popular and basically enabled by default on any device you might buy nowadays.

I personally use cloud storage but I would never actually upload anything I actually care about on it...
If you have to choose, I have a few suggestions.

### Don't trust non-encrypted solutions
Everybody has a Google account nowadays. If you forget your password, there is a way to recover it and get access to everything inside, including your Google Drive contents. As long as there is a password that can be changed or reset, your files are NOT encrypted and fully visible to anyone who has access to the Drive servers (Google or any other government agency that might want to take a peek).
Most cloud solutions work like this, and it's actually frightening how many people trust megacorporations to have all of their private information available unencrypted.

One encrypted solution I use is mega.nz.
While I can't be sure that the mega team isn't spying on me, at least they're hiding it well if they do.
Mega includes an encryption key with your account, which is not tied to your login information.
This means that if you lose your key, you also lost all of your files, there is absolutely no way to get them back, even if you change your account password.

Now, Mega is not open source, so you can never be sure that there isn't any backdoor, or that keys aren't stored together with your personal information, but at least it's something.

### Encrypt your data yourself
If you really need to trust Google, Apple or Amazon with your files, you can encrypt your files locally with the gpg command. This way, feds and big tech are going to need another password to actually access your private files.

It's really easy, just two commands mainly.

Encrypt:
```
gpg -c --cipher-algo AES256 secret.file
```

Decrypt:
```
gpg segret.file.gpg
```

If you need to encrypt a folder, you can compress it first:
```
tar -cf output.tar.gz secret-folder
```

Then encrypt your output.tar.gz archive as if it was a single file.

After decrypting it, you can extract your archive through this command:
```
tar -xf output.tar.gz
```

Check out Mental Outlaw's [video](https://invidio.us/M0O7vhvQW30) about this very topic.

## Local backups
This is the best way to backup your data.
You don't need to encrypt it if you have full physical access to your data, but you would still be vulnerable if it gets lost or stolen, so it's always better to keep it encrypted and safe.

Of course, if you have a backup hard drive always plugged in your PC, it's not really secure at all, since one could remotely execute a ransomware that encrypts everything in your PC, including all drives both internal and external, so you would get your backup encrypted with the original files voiding everything you've done.

This is why you should keep a GNU/Linux device that only serves backup purposes and is turned OFF most of the time. As long as no current runs through your CPU, your files are safe. You should only turn it on once a month and copy everything important over, so you have a safe and offline backup.

You could also use a USB stick or external hard drive, as long as you only plug them in your PC when necessary.
