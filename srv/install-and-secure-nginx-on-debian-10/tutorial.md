Authors: Giacinto Bertolacci
Published at: 2021-03-28T12:33Z
Tags: NGINX, Websites, Debian, Guides


# Install and Secure NGINX on Debian in just 3 Steps

Installing NGINX is a piece of cake! Really! Most tutorials out there just overly complicate the process. The tutorial below will show you how to install NGINX itself and add a free LetsEncrypt SSL certificate to serve your content over an encrypted HTTPS connection in just 3 simple steps. 


## Step 1 – Install the NGINX Dependency

The following tutorial is based on a plain *Debian 10 (Buster) Minimal* installation. No further dependencies are required to be preinstalled to follow this tutorial. All you need to follow this tutorial is SSH access to your [virtual server](https://www.alwyzon.com).

Installing NGINX on Debian is really easy as the official Debian repository already provides an NGINX package. Just run the following command to have NGINX installed and the default configuration created:

```
sudo apt update && sudo apt install nginx
```

To check if the installation was successful, run:

```
sudo service nginx status
```

If you see something that says `running`, then you are already nearly done. NGINX already has been installed on your virtual server and a default configuration was created in `/etc/nginx`.

## Step 2 – Prepare your Website

As a next step, you would usually upload the HTML, JS, CSS and related assets to your virtual server into the `/var/www/html` directory of your server. If you want, you can do that. Uploading files over SSH/SCP is as easy as `scp ./local/path/to/upload your-server.tld:/var/www/html` on Linux/macOS. Windows users can use Filezilla or PuTTY to achieve the same, but this is beyond the scope of this tutorial.

For demonstrative purposes of this tutorial, we will just create a minimal HTML page directly on this server. First, delete the existing `/var/www/html/` directory if there is any, then create a new one and create an `index.html` file:

```
rm -rf /var/www/html
mkdir -p /var/www/html
nano /var/www/html/index.html
```

You should see a  `nano` screen in your terminal now. A very simple text editor for the command line. Fill in the following HTML into that file and type `CTRL + X` to save and exit.

```
<!DOCTYPE html>
<html lang="en"><body>
  It's working! Wohooo!
</body></html>
```

Once that file is created, open the NGINX default site config (`nano /etc/nginx/sites-available/default`) and replace the content of this file with the following minimal configuration:

```
server {
  listen 80;
	root /var/www/html;
	index index.html;
	server_name your-domain.tld;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

Replace `your-domain.tld` with your domain, obviosuly. Then save and exit the editor via `CTRL + X`. Reload NGINX via `sudo service nginx reload` and you should already be able to reach your website over an unecnrypted HTTP connection.

## Step 3 – Request an SSL Certificate

[LetsEncrypt](https://letsencrypt.org/) is a non-profit project to promote the usage of encrypted traffic for all websites. Take that chance and deliver your website over a secure `https://`-connection too!

First, install `certbot`, a utility to fetch, retrieve and install LetsEncrypt certificates:

```
sudo apt install certbot
```

The certbot installer takes care already for most steps, including the creation of a `cronjob` to automatically renew your certificates. Then, all that's left is to tell `certbot` to fetch a certificate for your domain. **Note: you have to create DNS entries for your domain BEFORE you try to get an SSL certificate!** LetsEncrypt validates whether you authorized to get that certificate by checking if the domain points to the IP address from which the request originates.

```
sudo certbot --apache -d your-domain.tld -d www.your-domain.tld
```

Again, replace `your-domain.tld` with your actual domain! The command actually has two request certificates as you can see.

Finally, just tell NGINX to reload it's configuration and you're settled! `certbot` automatically installed the configuration for the newly fetched SSL certificates for you.

```
sudo service nginx reload
```

## Step 4 – Done!

That was it already! There are no more steps you have to follow. Your server already happily serves your website from <https://your-ip/>, or, if you already configured DNS entries for your domain, https://www.your-domain.tld/.

## Conclusion

Didn't I tell you it will be a piece of cake? Installing NGINX and requesting an SSL certificate barely takes more than a few minutes on Debian. But, now comes the hard part: you will have to design and implement your own unique website. 


## Further References

For further help, look at the NGINX website and their official documentaion, or for matters regarding your SSL certificate at the LetsEncrypt.org website.

- [NGINX Website](https://www.nginx.com)
- [NGINX Official Documentation](https://nginx.org/en/docs/)
- [LetsEncrypt Website](https://letsencrypt.org/de/)
- [Certbot Website](
