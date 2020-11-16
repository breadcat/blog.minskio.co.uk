---
title: "Selfhosted password manager"
date: 2018-09-26T11:30:00
tags: ["Guides", "Linux", "Selfhosted", "Software"]
---

Passwords in my experience are a fickle thing, on one hand you absolutely need long and complex passwords, different for every site you use, but remembering these unique and complex passwords is nigh impossible.
This issue is only made worse by brute forcing passwords being made easier every day as more and more datasets from large websites are leaked, showing patterns, schemes and weaknesses in unlikely places.
Password Managers bridge part of this gap, but then you're either relying on a third party to host these for you, or you're using a locally installed application which both come with their own collections of baggage and issues.

This would be where [Keeweb](https://github.com/keeweb/keeweb) comes in, as a single file, web based password manager that uses the standard KeePass format it seems like a nice fit for our requirements. As an added bonus, it can also be run as a [cross-platform application](https://github.com/keeweb/keeweb/releases/) which offers a nice offline alternative as well.

Helpfully, there are docker images available already which cover most of the leg work associated with grabbing the application and setting up a working webdav share. Assuming you have a working `docker` installation, simply run:

```
docker run -d -p 80:80 -e WEBDAV_USERNAME=username -e WEBDAV_PASSWORD=password -v $HOME/keeweb:/var/www/html/webdav viossat/keeweb-webdav
```

Or alternatively, in `docker-compose` notation:
```
keeweb:
  image: viossat/keeweb-webdav
  ports:
    - 80:80
  volumes:
    - $HOME/keeweb:/var/www/html/webdav
  environment:
    - WEBDAV_USERNAME=username
    - WEBDAV_PASSWORD=password
```

One point worth noting is that keeweb cannot create a database, so you'll need to create a blank database using [KeepassX](https://www.keepassx.org/downloads) or something similar.

When accessing your server on port 80 (First 80 in definitions is your *local access* port), you should now see the login screen.

You can select your webdav share under the *More...* menu item, and WebDAV as a sub option.
Your URL will be `server_address/webdav/database_name.xdbx`, and your username and password will be the values specified above.

One thing I had to change to get everything working perfectly was Settings > General > Storage > Save Method. This needed to be 'Overwrite kdbx file with PUT' otherwise you'll end up with a load of temporary files that make versioning interesting to say the least.

Lastly, if you're even considering accessing this application remotely you'll want to do this using HTTPS via a reverse proxy such as Caddy or Nginx.

**Update 2019-07-16**
I've recently moved this entire fragile stack over to Bitwarden, which in my experience works a lot better. You can see the docker-compose entry at [my dockerfiles repo](https://github.com/breadcat/Dockerfiles/blob/master/docker-compose.yml).