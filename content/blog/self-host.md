+++
title = "Self-hosting Extravaganza"
date = 2023-07-16
template = "blog-page.html"
[taxonomies]
tags = [ "advice", "foss", "privacy" ]
+++

Lately, more and more companies are putting their services behind paywalls, usage limits and closed APIs. Some examples are [Twitter](https://nitter.it/elonmusk/status/1675187969420828672) limiting the number of tweets a non-paying user can read, [Reddit](https://www.redditinc.com/blog/2023apiupdates) increasing their API price to an extent that's unbearable for any normal individual and [YouTube](https://libreddit.kavin.rocks/r/youtube/comments/14kmd07/youtube_cracking_down_on_if_youre_not_paying_them/) starting to block their service towards anyone using an adblock extension.

## There must be a better way
Luckily, I've been interested in [alternative front-ends](https://github.com/mendel5/alternative-front-ends) for a while. These services allow you to get the same (or better) functionality as their corporate counterpart without giving away any of your information in return. Some of these even offer their own free APIs.

Here's my favorite instances with respect to the service they provide:

| Service | PC                                         | Mobile                                                                          |
|---------|--------------------------------------------|---------------------------------------------------------------------------------|
| YouTube | [Invidious](https://y.com.sb/)             | [NewPipe](https://apt.izzysoft.de/fdroid/index/apk/org.polymorphicshade.newpipe)|
| Twitter | [Nitter](https://nitter.it)                | [Squawker](https://apt.izzysoft.de/fdroid/index/apk/org.ca.squawker)            |
| Reddit  | [LibReddit](https://libreddit.kavin.rocks) | [LibReddit](https://libreddit.kavin.rocks)                                      |
| Medium  | [Scribe](https://scribe.rip)               | [Scribe](https://scribe.rip/)                                                   |

## Drawbacks
Of course, this is not a perfect solution. There are a lot of problems to be discussed.

### Privacy
First and foremost, these instances do not make any profit. This is not a problem until you really think about it. Can you really trust a random developer offering a (paid) service for thousands of users out of their own kindness?
The answer is "probably yes", but are you willing to take this risk?

Instance admins could easily edit the upstream source code to make it so they can track their users indefinetly and sell usage data without them even realizing.
This is a given if you use any "normal" (not self-hosted) service, but the difference is big companies are _required_ by GDPR to protect collected user data in a certain way and keep them for a maximum set amount of time.

The same cannot be assured for individuals who apparently don't even make a profit for what they're doing.

### Scaling
This buzzword has become a meme in the programming world, but it's been shown how important it is to consider when dealing with large userbases that can grow exponentially without any warning.

Think about the amount of users who migrated to Mastodon immediately after Elon Musk acquired Twitter. Instance admins were used to having a couple hundred users, so hundred of thousands of new signups made a lot of popular instances slow down or even temporarily shut down while they migrated to new (and more expensive) hardware.

Anything public you use can be subject to this phenomenon, leading to poor user experience, as you'll be one of the many people wondering why your feed takes one minute to load.

## Fine, I'll do it myself

Since joining the world of minimalism, I had always considered Docker as a bloated way to run multiple virtual machines. I read about people complaining that even simple Python scripts were providing `Dockerfile` and `docker-compose.yml` files and I started seeing it as a bloaty way to achieve the same result.

Whenever I wanted to host anything by myself, I used to SSH into my VPS with password authentication (!!!)  and expose a public port for each service (!!!).
I used my public IP address to log into my services, so I had to resort to sending cleartext passwords through HTTP (!!!) since TLS was not an option.

Of course, this is possibly the most insecure way to host services on a public server, but I felt that was "secure enough" and nobody would ever be interested in hacking me (!!! &times; &infin;).

Nonetheless, I used to `cat /var/log/auth.log` to see all the failed login attempts, and pray that nobody actually got my password right.
Nowadays, I look back and laugh at my previous config; at least I'm (almost) sure that nobody actually managed to get in.

## The right way
Since I started my new job, I also began experimenting with Docker and found out it's not as bad as I thought it'd be. I will now let my previous config serve as the perfect example of how NOT to secure your VPS correctly for any self-hosting configuration.

### Ditch password authentication
First of all, password authentication. You'll be a lot safer as soon as you disable it.

Having it enabled means you're vulnerable to dictionary and bruteforce attacks. Also, if some new vulnerability is published, the password field is one more way the attacker could send a malicious string to get inside (see [the log4j incident](https://scribe.rip/geekculture/the-log4j-incident-explained-ed0ce6d36df2)).

A better way of logging into your VPS is through public key authentication.

First, generate a key on your own PC:
```
ssh-keygen -t ed25519 -a 100
```

This will create two files: `~/.ssh/id_ed25519.pub` and `~/.ssh/id_ed25519`

Now, use the following command to copy your key over to the VPS:

```
ssh-copy-id -i ~/.ssh/id_ed25519 <user>@<host>
```

At this point, if everything went correctly, just add or change the following line in `/etc/ssh/sshd_config`:
```
PasswordAuthentication no
```
At this point, you should be able to log into your VPS without the need to input your password, which is more secure as well as more convenient.

I keep the content of my public and private ssh key files saved as secure notes in my BitWarden account, so I can take them to any PC I want to access my VPS from.
People say this is bad practice (you should only have a key for each host), but I personally feel like it's not that big of a deal compared to the security mess I had going on before.

### Containerize your applications
Now that you have a safe way to SSH into your machine, you can start hosting your own services.

First, some terminology:
* `Dockerfile` files are like a list of ingredients. They contain every dependency needed to build a minimal operating system dedicated to running a program. They're used to build images.
* `Images` are like recipes. You can create some yourself from a Dockerfile or download them from an external repository. They can be instantiated as containers.
* `Containers` are like courses. You can instantiate multiple equal courses from the same image and you can actually eat (use) them! They can be managed through `docker-compose`.
* `docker-compose.yml` files are like menus. They're a convenient way to instantiate and deinstantiate multiple containers in a specific and reproducible configuration. If you're not a developer, you'll be mainly working on these files.

To get started with Docker, install `docker` and `docker-compose` via your package manager of choice. If you want an easy start, you can follow [this guide](https://docs.invidious.io/installation/#docker-compose-method-production) to host our own Invidious instance.

It's not that hard, but you might need to read the official [Docker Compose documentation](https://docs.docker.com/compose/) if something doesn't go as planned.

My advice is to generate an `hmac_key` using `pwgen 20 1` or `openssl rand -hex 20` and insert it in the correct spot inside `docker-compose.yml`.

Also, remove the `127.0.0.1:` part in the `ports` section since we don't have a reverse proxy set up (yet).

After you're done configuring, you can type `docker-compose up -d` to pull all required images and instantiate your containers, and `docker-compose down` if you want to stop and remove everything.

### Use a reverse proxy
If you've followed that guide correctly, you should now have two containers that communicate through a network. You can find out their names by running `docker ps -a`. Take note of the name of your main invidious container, which will be referred as `invidious` for the rest of this guide.

Problem is, you're still using an IP address and communicating in cleartext through HTTP! This means your ISP can read every single detail in every single request you make.

Luckily, there is a way to get a cool domain name for free that also happens to include free and auto-generated TLS certificates.

First, create an account on [DuckDNS](https://www.duckdns.org/) and set up a free domain.

Just make a new directory near the one you used for Invidious and create a new `docker-compose.yml`:
```
mkdir swag
cd swag
nano docker-compose.yml
```
You can paste and edit accordingly the lines in [this guide](https://docs.linuxserver.io/general/swag#creating-a-swag-container).

For example, instead of `DNSPLUGIN=cloudflare` you should have `DNSPLUGIN=duckdns`.

When you're done, start your container with `docker-compose up -d`. This will create the config folder in `/etc/config/swag` as well as a new network called `swag_default`.

Now we need to create a custom subdomain for Invidious. You can do it by creating the following file: `/etc/config/swag/nginx/proxy-confs/invidious.subdomain.conf` with this content:

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name y.*;

    include /config/nginx/ssl.conf;

    client_max_body_size 0;

    location / {
        include /config/nginx/proxy.conf;
        include /config/nginx/resolver.conf;
        set $upstream_app invidious;
        set $upstream_port 3000;
        set $upstream_proto http;
        proxy_pass $upstream_proto://$upstream_app:$upstream_port;
    }
}
```

Where:
* `server_name yt.*`: `yt` is the subdomain of choice;
* `set $upstream_app invidious;`: `invidious` is the name of the main Invidious container;
* `set $upstream_port 3000;`: `3000` is the Invidious port.

There's one last step remaining. Invidious and Swag are two separate containers, so they cannot communicate unless they're connected to the same network. You can connect Invidious to Swag's network with the following command, where `invidious` is the name of your main Invidious container.

```
docker network connect swag_default invidious
```

Finally, you can visit https://yt.&lt;your-domain&gt;.duckdns.org/ and check if you can access Invidious through HTTPS.

Note: now that you have a reverse proxy set up, you can remove your `ports:` section entirely from Invidious' `docker-compose.yml`.
You can do this because the containers are communicating internally to the `swag_default` network, without the need to expose any ports to the outside.
After you're done, remember to reload your configuration by running `docker-compose restart` in your Invidious folder.

Ideally, the only container with exposed ports in your VPS should be Swag exposing ports 80 (HTTP) and 443 (HTTPS).

## Conclusion
Self-hosting is not easy. It's been my [Camino de Santiago](https://wiki.froth.zone/wiki/Camino_de_Santiago): a long path of redemption for the sins I have committed in my young age.
Even if I made a lot of mistakes, in the end I've learned a lot about dev-ops and cybersecurity, as well as precious skills that proved themselves useful for my engineering job.

You can find a full list of self-hostable services [here](https://github.com/awesome-selfhosted/awesome-selfhosted)!
