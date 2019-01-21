# Listen The 443 Port

When I was debugging, I closed the process that occupied port 443 according to the online tutorial which said the `wss` service have to use port 443. Then it cause error when accessing file on my server. I have to say I can easily access the file before.

### Check the 443 port

On another computer terminal

`telnet xxx.xx.xx.xx 443`

Found that port 443 is unreachable.

### Check the server security group

Find the security group under the list of cloud servers, open the 443 channel in the inbound direction.

### Check the firewall

Ubuntu comes with a much simpler firewall configuration tool than iptables in its distribution: `ufw`

`apt-get install ufw`

Use ufw to open port 443

`ufw allow 443`

Now port 443 can be talented, but there is nothing under `netstat -antp | grep 443`, According to common sense, information containing `0.0.0.0:443` should be displayed, which means this port is being monitored.

### Check apache configuration

Now the 443 port is opened but still not being listened. Check the apache configuration files under `/etc/apache2/sites-available`, I found `Listen 443` in file. And I configured apache by [Certbot](https://certbot.eff.org/), the configuration had no problem.

### Violent resolution

I had no choice

`sudo /etc/init.d/apache2 restart`

Then 

`sudo Isof -iTCP -sTCP:LISTEN -P`

I saw `apache2 9785 www-data 4u IPv4 448556 0t0 TCP *:443 (LISTEN)`

Problem solved～

