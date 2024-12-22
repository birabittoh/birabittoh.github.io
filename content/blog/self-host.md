+++
title = "Self-hosting Extravaganza"
date = 2023-07-16
template = "blog-page.html"
[taxonomies]
tags = [ "advice", "foss", "privacy" ]
+++

Lately, more and more companies are putting their services behind paywalls, usage limits and closed APIs. Some examples are [Twitter](https://nitter.privacydev.net/elonmusk/status/1675187969420828672) limiting the number of tweets a non-paying user can read, [Reddit](https://www.redditinc.com/blog/2023apiupdates) increasing their API price to an extent that's unbearable for any normal individual and [YouTube](https://redlib.perennialte.ch/r/youtube/comments/14kmd07/youtube_cracking_down_on_if_youre_not_paying_them/) starting to block their service towards anyone using an adblock extension.

## There must be a better way
Luckily, I've been interested in [alternative front-ends](https://github.com/mendel5/alternative-front-ends) for a while. These services allow you to get the same (or better) functionality as their corporate counterpart without giving away any of your information in return. Some of these even offer their own free APIs.

Here's my favorite instances with respect to the service they provide:

| Service | PC                                       | Mobile                                                                           |
|---------|------------------------------------------|----------------------------------------------------------------------------------|
| YouTube | [Invidious](https://yewtu.be/)           | [Tubular](https://apt.izzysoft.de/fdroid/index/apk/org.polymorphicshade.tubular) |
| Twitter | [Nitter](https://nitter.privacydev.net/) | [Squawker](https://apt.izzysoft.de/fdroid/index/apk/org.ca.squawker)             |
| Reddit  | [RedLib](https://redlib.perennialte.ch/) | [RedLib](https://redlib.perennialte.ch/)                                         |
| Medium  | [Scribe](https://scribe.rip)             | [Scribe](https://scribe.rip/)                                                    |

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

Since joining the world of minimalism, I had always considered Docker as a bloated way to run multiple virtual machines. I read about people complaining that even simple Python scripts were providing `Dockerfile` and `compose.yml` files and I started seeing it as a bloaty way to achieve the same result.

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
```sh
ssh-keygen -t ed25519 -a 100
```

This will create two files: `~/.ssh/id_ed25519.pub` and `~/.ssh/id_ed25519`

Now, use the following command to copy your key over to the VPS:

```sh
ssh-copy-id -i ~/.ssh/id_ed25519 <user>@<ip>
```

At this point, if everything went correctly, just add or change the following lines in `/etc/ssh/sshd_config` on your VPS:
```sh
PasswordAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
At this point, you should be able to log into your VPS without the need to input your password, which is more secure AND more convenient.

I keep the content of my public and private ssh key files saved as secure notes in my BitWarden account, so I can take them to any PC I want to access my VPS from.
People say this is bad practice (you should only have a key for each host), but I personally feel like it's not that big of a deal compared to the security mess I had going on before.

### Containerize your applications
Now that you have a safe way to SSH into your machine, you can start hosting your own services.

First, some terminology:
* `Dockerfile` files are like a list of ingredients. They contain every dependency needed to build a minimal operating system dedicated to running a program. They're used to build images.
* `Images` are like recipes. You can create some yourself from a Dockerfile or download them from an external repository. They can be instantiated as containers.
* `Containers` are like courses. You can instantiate multiple equal courses from the same image and you can actually eat (use) them! They can be managed through `docker compose`.
* `compose.yml` files are like menus. They're a convenient way to instantiate and deinstantiate multiple containers in a specific and reproducible configuration. If you're not a developer, you'll be mainly working on these files.

To get started with Docker, install `docker` and `docker-compose` via your package manager of choice. Let's try hosting our own [RedLib](https://github.com/redlib-org/redlib) instance.

First, ssh into your VPS and clone the repo:

```sh
git clone https://github.com/redlib-org/redlib
cd redlib
```

Then, create your `.env` file:
```sh
cp .env.example .env
```

The redlib repo includes a `compose.yaml` file. You can start the service by running `docker compose up -d`, then test if you can reach your service by querying http://&lt;your-ip&gt;:8080.

When you want to stop the service, just run `docker compose down` to stop and delete all of its related containers.

### Use a reverse proxy
You should now have exposed port 8080 of your container to the internet.

Problem is, you're still using an IP address and communicating in cleartext through HTTP! This means your ISP can read every single detail in every single request you make.

Luckily, there is a way to get a cool domain name for free that also happens to include free and auto-generated TLS certificates.

First, create an account on [DuckDNS](https://www.duckdns.org/) and set up a free domain.

Just make a new directory near the one you used for RedLib and create a new `compose.yaml` file:
```
mkdir swag
cd swag
nano compose.yaml
```

Here's a good starting point for `compose.yaml`:
```yaml
services:
    swag:
        image: 'ghcr.io/linuxserver/swag'
        container_name: 'swag'
        cap_add:
            - 'NET_ADMIN'
        environment:
          PUID: '1000'
          PGID: '1000'
          TZ: 'Europe/Rome'
          URL: '<your-domain>.duckdns.org'
          SUBDOMAINS: 'wildcard'
          VALIDATION: 'duckdns'
          DUCKDNSTOKEN: '<your-token>'
          EMAIL: '<your-email>'
          ONLY_SUBDOMAINS: 'false'
          STAGING: 'false'
        volumes:
            - 'data:/config'
            - './www:/config/www'
            - './proxy-confs:/config/nginx/proxy-confs'
        ports:
            - '443:443'
            - '80:80'
        restart: 'unless-stopped'

volumes:
  data:
```

Check out [this guide](https://docs.linuxserver.io/general/swag#creating-a-swag-container) if you need more info.

When you're done, start your container with `docker compose up -d`. This will create the swag_data volume, as well as a new network called `swag_default` and two directories named `www` and `proxy-confs`.

Now we need to create a custom subdomain for RedLib. You can do it by creating a new file in `proxy-confs`:
```sh
cd proxy-confs
nano redlib.subdomain.conf
```

Then paste the following content:

```sql
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    http2 on;

    server_name r.*;

    include /config/nginx/ssl.conf;

    client_max_body_size 0;

    location / {
        include /config/nginx/proxy.conf;
        include /config/nginx/resolver.conf;
        set $upstream_app redlib;
        set $upstream_port 8080;
        set $upstream_proto http;
        proxy_pass $upstream_proto://$upstream_app:$upstream_port;
    }
}
```

Where:
* `server_name r.*`: `r` is your subdomain of choice;
* `set $upstream_app redlib;`: `redlib` is the name of the RedLib container;
* `set $upstream_port 8080;`: `8080` is the RedLib port.

There's one last step remaining. RedLib and Swag are two separate containers, so they cannot communicate unless they're connected to the same network.

RedLib's `compose.yaml` specifies a custom `redlib` network, while swag has its own `swag_default` network. Instead of the `redlib` network, we want our RedLib container to connect to `swag_default`.
To do this, cd to your `redlib` directory and make your own copy of `compose.yaml`:

```sh
cd ~/redlib
cp compose.yaml compose.custom.yaml
nano compose.custom.yaml
```

Now we can remove the `ports` section and add `swag_default` as an external network the container should connect to.

The final result should look something like this:
```yaml
services:
  redlib:
    image: quay.io/redlib/redlib:latest
    restart: always
    container_name: "redlib"
    user: nobody
    read_only: true
    security_opt:
      - no-new-privileges:true
      # - seccomp=seccomp-redlib.json
    cap_drop:
      - ALL
    env_file: .env
    networks:
      - swag_default # changed
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "--tries=1", "http://localhost:8080/settings"]
      interval: 5m
      timeout: 3s

networks:
  swag_default: # changed
    external: true # changed
```

Then, apply your updates by stopping the old configuration and starting the new one:
```sh
docker compose down
docker compose -f compose.custom.yaml up -d
```

Finally, you can visit https://r.&lt;your-domain&gt;.duckdns.org/ and check if you can access RedLib through HTTPS.

Ideally, the only container with exposed ports in your VPS should be Swag exposing ports 80 (HTTP) and 443 (HTTPS).

## Conclusion
Self-hosting is not easy. It's been my [Camino de Santiago](https://wiki.froth.zone/wiki/Camino_de_Santiago): a long path of redemption for the sins I have committed in my young age.
Even if I made a lot of mistakes, in the end I've learned a lot about dev-ops and cybersecurity, as well as precious skills that proved themselves useful for my engineering job.

You can find a full list of self-hostable services [here](https://github.com/awesome-selfhosted/awesome-selfhosted)!
